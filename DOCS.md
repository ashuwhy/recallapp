# RecallLabs Mobile App - Developer Documentation

## Table of Contents
1. [Project Overview](#project-overview)
2. [Tech Stack](#tech-stack)
3. [Architecture](#architecture)
4. [Setup & Installation](#setup--installation)
5. [Feature Modules](#feature-modules)
6. [Database Schema](#database-schema)
7. [API Integration](#api-integration)
8. [Offline-First Strategy](#offline-first-strategy)
9. [Testing](#testing)
10. [Deployment](#deployment)

## Project Overview
RecallLabs mobile app is an offline-capable flashcard review platform for students preparing for exams like NEET. The MVP targets NEET Biology (Class 11, Unit 1) with 100 students.

**Core User Flow:**
1. Student logs in via email + OTP
2. App syncs today's deck (20-40 cards) from backend
3. Student reviews cards offline with swipe gestures (Easy/Hard/Again)
4. Reviews sync to backend when online to update spaced repetition schedule

## Tech Stack
| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| Framework | Expo | 52+ | React Native framework |
| Language | TypeScript | 5.3+ | Type-safe development |
| Navigation | Expo Router | Latest | File-based routing |
| Local DB | WatermelonDB | 0.27+ | Offline data storage |
| Server Sync | React Query | 5.0+ | API calls & caching |
| State Mgmt | Zustand | 4.5+ | Global state |
| Auth | Expo SecureStore | Latest | Token storage |
| UI Library | React Native Paper | 5.12+ | Material Design components |

## Architecture

### Folder Structure
```
recallapp/
├── app/                    # Expo Router screens (file-based routing)
│   ├── (auth)/            # Auth flow screens
│   │   ├── login.tsx
│   │   └── onboarding.tsx
│   ├── (tabs)/            # Main app tabs
│   │   ├── home.tsx       # Daily deck screen
│   │   ├── progress.tsx   # Progress tracking
│   │   └── profile.tsx
│   └── _layout.tsx        # Root layout
├── src/
│   ├── features/          # Feature modules
│   │   ├── auth/
│   │   │   ├── api/       # Auth API calls
│   │   │   ├── hooks/     # useLogin, useAuth
│   │   │   ├── types/     # TypeScript types
│   │   │   └── utils/     # Helper functions
│   │   ├── cards/         # Card display & review
│   │   ├── sync/          # Offline sync logic
│   │   └── progress/      # Progress tracking
│   ├── database/          # WatermelonDB
│   │   ├── models/        # Card, Review, SyncQueue
│   │   ├── schema.ts      # Database schema
│   │   └── migrations/    # Schema migrations
│   ├── api/               # API client
│   │   ├── client.ts      # Axios instance
│   │   └── endpoints/     # API endpoint functions
│   ├── components/        # Shared UI components
│   ├── hooks/             # Custom React hooks
│   ├── utils/             # Helper functions
│   └── types/             # Shared TypeScript types
├── assets/                # Images, fonts
└── tests/                 # Test files
```

### Design Patterns
- **Feature-Based Architecture**: Each feature is self-contained with its own API, hooks, types, and utils
- **Offline-First**: All user actions work offline, sync happens in background
- **Repository Pattern**: Database access abstracted through repository functions
- **Custom Hooks**: Business logic encapsulated in reusable hooks

## Setup & Installation

### Prerequisites
- Node.js 20+ (LTS)
- npm or yarn
- Expo CLI (`npm install -g expo-cli`)
- iOS Simulator (Mac) or Android Emulator

### Steps
```
# Clone repository
git clone https://github.com/ashuwhy/recallapp.git
cd recallapp

# Install dependencies
yarn install

# Set up environment variables
cp .env.example .env
# Edit .env with your API URL and secrets

# Start development server
npx expo start

# Run on iOS (Mac only)
npx expo run:ios

# Run on Android
npx expo run:android
```

### Environment Variables
```
EXPO_PUBLIC_API_URL=http://localhost:8000/api/v1
EXPO_PUBLIC_SENTRY_DSN=your_sentry_dsn
```

## Feature Modules

### 1. Authentication (`/src/features/auth`)
**Purpose**: Handle user login, token management, and session persistence.

**Files**:
- `api/authApi.ts`: Login, refresh token endpoints
- `hooks/useAuth.ts`: Auth state and methods
- `hooks/useLogin.ts`: Login form logic

**Flow**:
1. User enters email on login screen
2. Backend sends OTP to email
3. User enters OTP
4. App receives JWT (access + refresh tokens)
5. Tokens stored in Expo SecureStore
6. User profile fetched and stored in Zustand

### 2. Cards & Review (`/src/features/cards`)
**Purpose**: Display flashcards and capture review ratings.

**Components**:
- `ReviewCard.tsx`: Swipeable card with front/back flip
- `DeckScreen.tsx`: Container for daily deck
- `RatingButtons.tsx`: Easy/Hard/Again buttons

**Review Flow**:
1. Fetch cards from local WatermelonDB
2. Display card front
3. User taps to reveal back
4. User swipes or taps rating (0=Again, 1=Hard, 2=Easy)
5. Review saved to local `reviews` table
6. Sync queue updated for background upload

### 3. Sync (`/src/features/sync`)
**Purpose**: Manage offline/online sync of reviews and deck updates.

**Files**:
- `syncService.ts`: Sync logic (upload reviews, download deck)
- `hooks/useSyncStatus.ts`: Online/offline detection
- `SyncQueue.model.ts`: WatermelonDB model for pending sync items

**Sync Strategy**:
- Reviews queued locally when offline
- Background sync triggered when online (network change listener)
- Retry with exponential backoff on failures
- Conflict resolution: server always wins for deck updates

### 4. Progress (`/src/features/progress`)
**Purpose**: Show student progress, streak, mastery levels.

**Metrics**:
- Streak count (consecutive days reviewed)
- Cards reviewed today/total
- Mastery per chapter (% cards with interval > 7 days)

## Database Schema (WatermelonDB)

### Models
```
// Card.model.ts
@model('cards')
class Card extends Model {
  @field('card_id') cardId: string;           // Backend ID
  @field('chapter_id') chapterId: string;
  @field('type') type: 'flashcard' | 'true_false';
  @field('front') front: string;
  @field('back') back: string;
  @field('due_date') dueDate: number;         // Timestamp
  @field('easiness_factor') easinessFactor: number;
  @field('interval_days') intervalDays: number;
  @field('repetitions') repetitions: number;
}

// Review.model.ts
@model('reviews')
class Review extends Model {
  @field('card_id') cardId: string;
  @field('rating') rating: number;            // 0, 1, 2
  @field('reviewed_at') reviewedAt: number;   // Timestamp
  @field('synced') synced: boolean;           // False until uploaded
}

// SyncQueue.model.ts
@model('sync_queue')
class SyncQueue extends Model {
  @field('type') type: 'review' | 'deck_update';
  @json('payload') payload: any;
  @field('retry_count') retryCount: number;
  @field('created_at') createdAt: number;
}
```

### Migrations
- Create migration for each schema change
- Example: `src/database/migrations/001_initial_schema.ts`

## API Integration

### Client Setup (`src/api/client.ts`)
```
import axios from 'axios';
import * as SecureStore from 'expo-secure-store';

const apiClient = axios.create({
  baseURL: process.env.EXPO_PUBLIC_API_URL,
  timeout: 30000,
});

// Add auth token to requests
apiClient.interceptors.request.use(async (config) => {
  const token = await SecureStore.getItemAsync('accessToken');
  if (token) {
    config.headers.Authorization = `Bearer ${token}`;
  }
  return config;
});

// Handle token refresh on 401
apiClient.interceptors.response.use(
  (response) => response,
  async (error) => {
    if (error.response?.status === 401) {
      // Attempt token refresh
      const refreshToken = await SecureStore.getItemAsync('refreshToken');
      const { data } = await axios.post(`${baseURL}/auth/refresh`, { refreshToken });
      await SecureStore.setItemAsync('accessToken', data.accessToken);
      // Retry original request
      return apiClient.request(error.config);
    }
    throw error;
  }
);
```

### React Query Hooks (`src/features/cards/hooks/useDailyDeck.ts`)
```
import { useQuery } from '@tanstack/react-query';
import { fetchDailyDeck } from '../api/cardsApi';

export const useDailyDeck = () => {
  return useQuery({
    queryKey: ['dailyDeck'],
    queryFn: fetchDailyDeck,
    staleTime: 1000 * 60 * 60, // 1 hour
    retry: 3,
    retryDelay: (attempt) => Math.min(1000 * 2 ** attempt, 30000),
  });
};
```

## Offline-First Strategy

### Core Principles
1. **Read Offline**: All card data stored locally in WatermelonDB
2. **Write Offline**: Reviews saved locally, synced later
3. **Optimistic UI**: Instant feedback, resolve conflicts later
4. **Background Sync**: Upload when online, silent to user

### Implementation
- **Network Detection**: Use `@react-native-community/netinfo`
- **Sync Trigger**: Network change listener + periodic check (every 5 min)
- **Conflict Resolution**: Server state always wins for card content; merge reviews chronologically

## Testing

### Unit Tests (Jest)
```
npm run test
```

Test files co-located with source:
- `src/features/cards/utils/__tests__/sm2.test.ts`
- `src/api/__tests__/client.test.ts`

### Integration Tests (Detox)
```
npm run test:e2e:ios
npm run test:e2e:android
```

Critical flows to test:
- Login → Deck sync → Review card → Logout
- Offline review → Go online → Verify sync

## Deployment

### Build for Production
```
# iOS
eas build --platform ios --profile production

# Android
eas build --platform android --profile production
```

### OTA Updates (Expo)
```
eas update --branch production
```

### Environment Management
- **Development**: Local API, debug mode
- **Staging**: Staging API, Sentry enabled
- **Production**: Production API, analytics enabled

---

## Common Tasks

### Add New Card Type
1. Update `Card.model.ts` with new type
2. Create migration: `npx expo-sqlite generate-migration add_<type>_type`
3. Update `ReviewCard.tsx` to render new type
4. Add backend schema change

### Debug Offline Sync
1. Check network status: `NetInfo.fetch()`
2. Query sync queue: `database.get('sync_queue').query().fetch()`
3. Check Sentry for sync errors
4. Manually trigger sync: `await syncService.uploadPendingReviews()`

## FAQ

**Q: How does spaced repetition scheduling work?**  
A: Backend computes next due date using SM-2 algorithm when you submit a review. Mobile app fetches cards where `due_date <= today`.

**Q: What happens if a review fails to sync?**  
A: It stays in sync queue, retries with exponential backoff (max 3 attempts). After 3 failures, logged to Sentry for manual review.

**Q: Can students review the same card multiple times per day?**  
A: No, once reviewed, card's `due_date` updates to a future date. Only cards due today appear in daily deck.
