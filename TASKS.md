# RecallApp Tasks

## Epic 1: Project Setup
- [ ] **APP-001**: Initialize Expo project with TypeScript
- [ ] **APP-002**: Configure Expo Router file-based navigation
- [ ] **APP-003**: Set up WatermelonDB with initial schema
- [ ] **APP-004**: Configure environment variables (.env)
- [ ] **APP-005**: Set up Zustand store for auth state
- [ ] **APP-006**: Configure Axios client with interceptors
- [ ] **APP-007**: Set up React Query provider
- [ ] **APP-008**: Add ESLint, Prettier, TypeScript configs
- [ ] **APP-009**: Create folder structure (features, components, hooks)
- [ ] **APP-010**: Set up Sentry for error tracking

## Epic 2: Authentication
- [ ] **APP-011**: Create Login screen UI (email input)
- [ ] **APP-012**: Build OTP verification screen
- [ ] **APP-013**: Implement `authApi.ts` (login, verify OTP)
- [ ] **APP-014**: Create `useAuth` hook with Zustand
- [ ] **APP-015**: Implement token storage (SecureStore)
- [ ] **APP-016**: Add token refresh logic in Axios interceptor
- [ ] **APP-017**: Build Onboarding screen (first-time user)
- [ ] **APP-018**: Add logout functionality
- [ ] **APP-019**: Write unit tests for auth hooks
- [ ] **APP-020**: Add protected route wrapper

## Epic 3: Cards & Review
- [ ] **APP-021**: Create Card WatermelonDB model
- [ ] **APP-022**: Build ReviewCard component (flip animation)
- [ ] **APP-023**: Implement swipe gestures (react-native-gesture-handler)
- [ ] **APP-024**: Create RatingButtons component (Easy/Hard/Again)
- [ ] **APP-025**: Build DeckScreen to display daily cards
- [ ] **APP-026**: Implement `useDailyDeck` React Query hook
- [ ] **APP-027**: Add loading/empty/error states for deck
- [ ] **APP-028**: Create Review WatermelonDB model
- [ ] **APP-029**: Implement local review save logic
- [ ] **APP-030**: Add review submission to sync queue
- [ ] **APP-031**: Build CardFlipAnimation component
- [ ] **APP-032**: Write integration tests for review flow

## Epic 4: Offline Sync
- [ ] **APP-033**: Set up NetInfo for network detection
- [ ] **APP-034**: Create SyncQueue WatermelonDB model
- [ ] **APP-035**: Build `syncService.ts` (upload/download logic)
- [ ] **APP-036**: Implement background sync trigger (network listener)
- [ ] **APP-037**: Add exponential backoff retry logic
- [ ] **APP-038**: Create `useSyncStatus` hook
- [ ] **APP-039**: Build offline indicator banner UI
- [ ] **APP-040**: Implement conflict resolution for deck updates
- [ ] **APP-041**: Add sync progress indicator
- [ ] **APP-042**: Write tests for sync service

## Epic 5: Progress & Polish
- [ ] **APP-043**: Build ProgressScreen UI (streak, stats)
- [ ] **APP-044**: Calculate mastery metrics from local DB
- [ ] **APP-045**: Add streak counter logic
- [ ] **APP-046**: Implement daily reminder notification
- [ ] **APP-047**: Build Profile screen (student info, logout)
- [ ] **APP-048**: Add accessibility labels and screen reader support
