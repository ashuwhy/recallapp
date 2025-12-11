# RecallApp Development Tasks

> **Status Legend**: ✅ Complete | 🚧 In Progress | ⏳ Pending | 🔴 Blocked

---

## 📦 Epic 1: Project Setup & Foundation

### Core Setup
- [x] **APP-001**: Initialize Expo project with TypeScript
  - ✅ Expo SDK 54 configured
  - ✅ TypeScript 5.9.2 installed
  - ✅ Basic app structure created

- [x] **APP-002**: Configure Expo Router file-based navigation
  - ✅ `app/_layout.tsx` with Stack navigator
  - ✅ `app/index.tsx` home screen
  - ✅ File-based routing working

- [x] **APP-008**: Configure TypeScript and build tools
  - ✅ `tsconfig.json` configured (strict mode)
  - ✅ `babel.config.js` with reanimated plugin
  - ✅ `metro.config.js` with NativeWind integration

- [x] **APP-STYLING**: Set up NativeWind v4 (Tailwind CSS)
  - ✅ NativeWind 4.2.1 installed
  - ✅ `tailwind.config.js` configured
  - ✅ `global.css` created
  - ✅ Basic styling working in components

### Database & State Management
- [ ] **APP-003**: Set up WatermelonDB with initial schema
  - [ ] Install `@nozbe/watermelondb` and SQLite adapter
  - [ ] Create database instance in `src/database/index.ts`
  - [ ] Define initial models: User, Card, Review, SyncQueue
  - [ ] Create migrations folder structure

- [ ] **APP-005**: Set up Zustand store for auth state
  - [ ] Install `zustand` and `zustand-persist`
  - [ ] Create `src/store/authStore.ts`
  - [ ] Define auth state interface (user, token, isAuthenticated)
  - [ ] Add persistence middleware

### API & Networking
- [ ] **APP-004**: Configure environment variables (.env)
  - [ ] Create `.env.example` template
  - [ ] Install `expo-constants` for env access
  - [ ] Set up `EXPO_PUBLIC_API_URL` config
  - [ ] Add `.env` to `.gitignore`

- [ ] **APP-006**: Configure Axios client with interceptors
  - [ ] Install `axios`
  - [ ] Create `src/api/client.ts` with base config
  - [ ] Add request interceptor (attach JWT token)
  - [ ] Add response interceptor (handle 401, retry logic)
  - [ ] Set 30s timeout

- [ ] **APP-007**: Set up React Query provider
  - [ ] Install `@tanstack/react-query`
  - [ ] Create `src/providers/QueryProvider.tsx`
  - [ ] Wrap app with QueryClientProvider
  - [ ] Configure default options (retry, staleTime)

### Code Quality & Structure
- [ ] **APP-009**: Create folder structure (features, components, hooks)
  - [ ] Create `src/features/` (auth, cards, review, progress)
  - [ ] Create `src/components/` (shared UI components)
  - [ ] Create `src/hooks/` (custom React hooks)
  - [ ] Create `src/utils/` (helper functions)
  - [ ] Create `src/types/` (TypeScript interfaces)
  - [ ] Create `src/constants/` (app constants)

- [ ] **APP-LINT**: Add ESLint and Prettier
  - [ ] Install ESLint with Expo config
  - [ ] Install Prettier
  - [ ] Create `.eslintrc.js` and `.prettierrc`
  - [ ] Add lint/format scripts to package.json
  - [ ] Set up pre-commit hooks (optional)

- [ ] **APP-010**: Set up Sentry for error tracking
  - [ ] Install `@sentry/react-native`
  - [ ] Configure Sentry in `app/_layout.tsx`
  - [ ] Add error boundary component
  - [ ] Set up release tracking

---

## 🔐 Epic 2: Authentication

### UI Components
- [ ] **APP-011**: Create Login screen UI (email input)
  - [ ] Create `app/(auth)/login.tsx`
  - [ ] Use TextInput component from `@components`
  - [ ] Add email validation
  - [ ] Style with NativeWind (minimalistic, sleek design)
  - [ ] Add loading state

- [ ] **APP-012**: Build OTP verification screen
  - [ ] Create `app/(auth)/verify-otp.tsx`
  - [ ] 6-digit OTP input (use TextInput from components)
  - [ ] Auto-submit on complete
  - [ ] Resend OTP button with countdown
  - [ ] Error handling UI

- [ ] **APP-017**: Build Onboarding screen (first-time user)
  - [ ] Create `app/(auth)/onboarding.tsx`
  - [ ] Multi-step intro screens
  - [ ] Skip/Next navigation
  - [ ] Store onboarding completion in SecureStore

### API & Logic
- [ ] **APP-013**: Implement `authApi.ts` (login, verify OTP)
  - [ ] Create `src/api/authApi.ts`
  - [ ] `login(email: string)` → returns { sessionId }
  - [ ] `verifyOTP(sessionId, otp)` → returns { token, user }
  - [ ] Add error handling with user-friendly messages

- [ ] **APP-014**: Create `useAuth` hook with Zustand
  - [ ] Create `src/hooks/useAuth.ts`
  - [ ] Connect to authStore
  - [ ] Expose: `login`, `logout`, `user`, `isAuthenticated`
  - [ ] Add loading/error states

- [ ] **APP-015**: Implement token storage (SecureStore)
  - [ ] Install `expo-secure-store`
  - [ ] Create `src/utils/storage.ts`
  - [ ] `storeToken(token)` and `getToken()`
  - [ ] `removeToken()` for logout
  - [ ] Auto-load token on app start

- [ ] **APP-016**: Add token refresh logic in Axios interceptor
  - [ ] Detect 401 responses
  - [ ] Call refresh endpoint (if available)
  - [ ] Retry original request
  - [ ] Redirect to login if refresh fails

- [ ] **APP-018**: Add logout functionality
  - [ ] Clear SecureStore token
  - [ ] Reset Zustand auth state
  - [ ] Clear React Query cache
  - [ ] Navigate to login screen

- [ ] **APP-020**: Add protected route wrapper
  - [ ] Create `src/components/ProtectedRoute.tsx`
  - [ ] Check auth state
  - [ ] Redirect to login if not authenticated
  - [ ] Wrap protected screens

### Testing
- [ ] **APP-019**: Write unit tests for auth hooks
  - [ ] Test `useAuth` hook
  - [ ] Mock SecureStore and API calls
  - [ ] Test login/logout flows
  - [ ] Use React Native Testing Library

---

## 🃏 Epic 3: Cards & Review System

### Database Models
- [ ] **APP-021**: Create Card WatermelonDB model
  - [ ] Define Card schema (id, front, back, chapter, difficulty, etc.)
  - [ ] Create `src/database/models/Card.ts`
  - [ ] Add relations (to Review, Chapter)
  - [ ] Add indexes for queries

- [ ] **APP-028**: Create Review WatermelonDB model
  - [ ] Define Review schema (id, cardId, rating, timestamp, synced)
  - [ ] Create `src/database/models/Review.ts`
  - [ ] Add relation to Card
  - [ ] Add sync status field

### UI Components
- [ ] **APP-022**: Build ReviewCard component (flip animation)
  - [ ] Create `src/components/ReviewCard.tsx`
  - [ ] Use `react-native-reanimated` for flip
  - [ ] Show front/back content
  - [ ] Tap to flip gesture
  - [ ] Style with NativeWind

- [ ] **APP-031**: Build CardFlipAnimation component
  - [ ] Extract flip logic to reusable component
  - [ ] Use `useSharedValue` and `useAnimatedStyle`
  - [ ] Smooth 3D flip transition
  - [ ] Support both directions

- [ ] **APP-023**: Implement swipe gestures (react-native-gesture-handler)
  - [ ] Install `react-native-gesture-handler`
  - [ ] Add swipe left/right detection
  - [ ] Animate card dismissal
  - [ ] Trigger rating on swipe (Easy/Hard)

- [ ] **APP-024**: Create RatingButtons component (Easy/Hard/Again)
  - [ ] Create `src/components/RatingButtons.tsx`
  - [ ] Three buttons with icons
  - [ ] Haptic feedback on press
  - [ ] Disabled state during submission
  - [ ] Style with NativeWind (premium aesthetics)

### Screens & Data Fetching
- [ ] **APP-025**: Build DeckScreen to display daily cards
  - [ ] Create `app/(tabs)/deck.tsx`
  - [ ] Fetch cards from WatermelonDB
  - [ ] Render ReviewCard in FlatList
  - [ ] Handle empty state (no cards)
  - [ ] Pull-to-refresh for sync

- [ ] **APP-026**: Implement `useDailyDeck` React Query hook
  - [ ] Create `src/hooks/useDailyDeck.ts`
  - [ ] Fetch from `/daily-deck` API endpoint
  - [ ] Store cards in WatermelonDB
  - [ ] Return local DB query as fallback
  - [ ] Handle offline scenario

- [ ] **APP-027**: Add loading/empty/error states for deck
  - [ ] Loading skeleton component
  - [ ] Empty state with illustration
  - [ ] Error state with retry button
  - [ ] Use React Query states

### Review Logic
- [ ] **APP-029**: Implement local review save logic
  - [ ] Create `src/services/reviewService.ts`
  - [ ] Save review to WatermelonDB
  - [ ] Update card difficulty based on rating
  - [ ] Calculate next review date (spaced repetition)

- [ ] **APP-030**: Add review submission to sync queue
  - [ ] Create SyncQueue model (if not done)
  - [ ] Queue review for sync when offline
  - [ ] Mark as synced after successful upload
  - [ ] Handle sync failures

### Testing
- [ ] **APP-032**: Write integration tests for review flow
  - [ ] Test card flip interaction
  - [ ] Test rating submission
  - [ ] Test offline queue
  - [ ] Mock WatermelonDB and API

---

## 🔄 Epic 4: Offline Sync & Network Handling

### Network Detection
- [ ] **APP-033**: Set up NetInfo for network detection
  - [ ] Install `@react-native-community/netinfo`
  - [ ] Create `src/hooks/useNetworkStatus.ts`
  - [ ] Listen to network state changes
  - [ ] Expose `isOnline` boolean

### Sync Infrastructure
- [ ] **APP-034**: Create SyncQueue WatermelonDB model
  - [ ] Define SyncQueue schema (id, type, payload, status, retries)
  - [ ] Create `src/database/models/SyncQueue.ts`
  - [ ] Add indexes for pending items

- [ ] **APP-035**: Build `syncService.ts` (upload/download logic)
  - [ ] Create `src/services/syncService.ts`
  - [ ] `syncReviews()` - upload pending reviews
  - [ ] `syncDeck()` - download latest deck
  - [ ] Handle conflicts (server wins or merge strategy)

- [ ] **APP-036**: Implement background sync trigger (network listener)
  - [ ] Auto-trigger sync when network comes online
  - [ ] Use NetInfo listener
  - [ ] Debounce rapid network changes
  - [ ] Show sync indicator

- [ ] **APP-037**: Add exponential backoff retry logic
  - [ ] Implement retry with backoff (1s, 2s, 4s, 8s)
  - [ ] Max 3 retry attempts
  - [ ] Update SyncQueue retry count
  - [ ] Mark as failed after max retries

### UI Indicators
- [ ] **APP-038**: Create `useSyncStatus` hook
  - [ ] Track sync state (idle, syncing, error)
  - [ ] Expose sync progress percentage
  - [ ] Return pending items count

- [ ] **APP-039**: Build offline indicator banner UI
  - [ ] Create `src/components/OfflineBanner.tsx`
  - [ ] Show at top of screen when offline
  - [ ] Auto-hide when online
  - [ ] Style with NativeWind (subtle, non-intrusive)

- [ ] **APP-041**: Add sync progress indicator
  - [ ] Show sync progress in header or banner
  - [ ] Display "Syncing X items..."
  - [ ] Show success/error states

### Conflict Resolution
- [ ] **APP-040**: Implement conflict resolution for deck updates
  - [ ] Compare local vs server timestamps
  - [ ] Server wins strategy (or user choice)
  - [ ] Merge non-conflicting changes
  - [ ] Notify user of conflicts

### Testing
- [ ] **APP-042**: Write tests for sync service
  - [ ] Test offline queue
  - [ ] Test retry logic
  - [ ] Test conflict resolution
  - [ ] Mock network state

---

## 📊 Epic 5: Progress Tracking & Polish

### Progress Screen
- [ ] **APP-043**: Build ProgressScreen UI (streak, stats)
  - [ ] Create `app/(tabs)/progress.tsx`
  - [ ] Display streak counter
  - [ ] Show mastery per chapter
  - [ ] Cards reviewed today/week
  - [ ] Use charts (if needed)

- [ ] **APP-044**: Calculate mastery metrics from local DB
  - [ ] Query reviews from WatermelonDB
  - [ ] Calculate accuracy percentage
  - [ ] Group by chapter/topic
  - [ ] Cache calculations

- [ ] **APP-045**: Add streak counter logic
  - [ ] Track consecutive days with reviews
  - [ ] Reset on missed day
  - [ ] Store in WatermelonDB
  - [ ] Update daily

### Profile & Settings
- [ ] **APP-047**: Build Profile screen (student info, logout)
  - [ ] Create `app/(tabs)/profile.tsx`
  - [ ] Display user info (name, email)
  - [ ] Logout button
  - [ ] Settings section (notifications, theme)

### Notifications
- [ ] **APP-046**: Implement daily reminder notification
  - [ ] Install `expo-notifications`
  - [ ] Request permissions
  - [ ] Schedule daily reminder (user-configurable time)
  - [ ] Handle notification tap (open app)

### Accessibility & Polish
- [ ] **APP-048**: Add accessibility labels and screen reader support
  - [ ] Add `accessibilityLabel` to all interactive elements
  - [ ] Test with VoiceOver (iOS) and TalkBack (Android)
  - [ ] Ensure proper heading hierarchy
  - [ ] Minimum 44x44 touch targets
  - [ ] Support dynamic type

- [ ] **APP-049**: Dark mode support
  - [ ] Use Tailwind `dark:` modifier
  - [ ] Add theme toggle in settings
  - [ ] Persist theme preference
  - [ ] Test all screens in dark mode

- [ ] **APP-050**: Performance optimization
  - [ ] Optimize FlatList with `getItemLayout`
  - [ ] Memoize expensive components
  - [ ] Use `react-native-fast-image` for images
  - [ ] Remove console.logs in production
  - [ ] Profile with React DevTools

---

## 📝 Notes

### Completed ✅
- Expo project initialized with TypeScript
- Expo Router configured
- TypeScript strict mode enabled
- NativeWind v4 (Tailwind) set up
- Babel config with reanimated plugin
- Basic app structure and routing

### Next Priorities
1. Set up folder structure (`src/` directory)
2. Configure environment variables
3. Set up WatermelonDB
4. Create Zustand auth store
5. Build login screen

### Dependencies
- WatermelonDB setup required before card/review features
- Auth must be complete before protected routes
- Sync service depends on WatermelonDB models
- Progress screen needs review data
