# Tic-Tac-Toe React Native - Detailed Technical Documentation

## 🏗️ Architecture Overview

This React Native application follows a well-structured, modular architecture with clear separation of concerns. The project is built using modern React patterns with TypeScript for type safety and Expo for simplified development workflow.

### Core Architecture Patterns

1. **Component-Based Architecture**: Reusable UI components with clear interfaces
2. **Context Pattern**: Global state management for settings and app configuration
3. **Custom Hooks**: Encapsulated logic for sounds, settings, and game utilities
4. **Screen-Based Navigation**: Stack navigation with React Navigation
5. **Type-Safe Development**: Comprehensive TypeScript integration

## 📁 Detailed File Structure Analysis

### `/src` Directory Breakdown

#### `/components` - UI Component Library
```
components/
├── index.ts                 # Component barrel exports
├── app-bootstrap/          
│   └── app-bootstrap.tsx   # Font loading and app initialization
├── board/
│   ├── board.tsx           # Main game board component
│   ├── board-line.tsx      # Winning line animation
│   └── board.styles.ts     # Board styling
├── button/
│   ├── button.tsx          # Custom button component
│   └── button.styles.ts    # Button styling
├── gradient-background/
│   └── gradient-background.tsx # Consistent app background
└── text/
    └── text.tsx            # Styled text component with custom fonts
```

**Key Components Analysis:**

1. **AppBootstrap Component** (`app-bootstrap.tsx`)
   - Handles font loading with `expo-font`
   - Manages splash screen visibility
   - Prevents app rendering until fonts are loaded
   - Uses callback pattern for performance optimization

2. **Board Component** (`board.tsx`)
   - Renders 3x3 game grid
   - Handles cell press events
   - Manages disabled state
   - Integrates winning line animations
   - Dynamic sizing based on screen dimensions

3. **Button Component** (`button.tsx`)
   - Reusable button with consistent styling
   - TouchableOpacity-based for haptic feedback
   - Customizable styling via props

#### `/screens` - Application Screens
```
screens/
├── index.ts                # Screen barrel exports
├── home/
│   ├── home.tsx           # Main menu screen
│   └── home.styles.ts     # Home screen styling
├── settings/
│   ├── settings.tsx       # Settings configuration screen
│   └── settings.styles.ts # Settings styling
└── single-player-game/
    ├── single-player-game.tsx # Main game screen
    └── single-player-game.styles.ts # Game styling
```

**Screen Analysis:**

1. **Home Screen** (`home.tsx`)
   - Main navigation hub
   - Navigation to SinglePlayerGame and Settings
   - Placeholder buttons for future features (MultiPlayer, Login)
   - Logo display with proper asset management

2. **Single Player Game Screen** (`single-player-game.tsx`)
   - Core game logic implementation
   - State management for board, turn, and game statistics
   - AI integration with difficulty levels
   - Sound and haptic feedback integration
   - Real-time score tracking (wins/losses/draws)
   - Modal for game end states

3. **Settings Screen** (`settings.tsx`)
   - Difficulty selection interface
   - Sound/haptic toggle switches
   - Persistent settings via AsyncStorage
   - Dynamic UI based on current settings

#### `/utils` - Game Logic and Utilities
```
utils/
├── index.ts               # Utility barrel exports
├── board.ts              # Board state management
├── player.ts             # AI minimax algorithm
├── types.ts              # TypeScript type definitions
├── colors.ts             # Color constants
└── useSounds.ts          # Sound management hook
```

**Utility Analysis:**

1. **Board Utils** (`board.ts`)
   ```typescript
   // Key Functions:
   - isEmpty(state): Checks if board is empty
   - isFull(state): Checks if board is completely filled
   - getAvailableMoves(state): Returns available cell indices
   - isTerminal(state): Checks for win/draw conditions
   - printFormattedBoard(state): Debug utility for console logging
   ```

2. **Player AI** (`player.ts`)
   ```typescript
   // Minimax Algorithm Implementation:
   - getBestMove(): Main AI decision function
   - Recursive minimax with alpha-beta pruning concepts
   - Depth-limited search for difficulty levels
   - Random move selection from equally good options
   - Performance optimization with memoization patterns
   ```

3. **Sound Hook** (`useSounds.ts`)
   ```typescript
   // Audio Management:
   - Preloads all sound files on component mount
   - Manages Audio.Sound instances with refs
   - Integrates haptic feedback
   - Conditional playback based on settings
   - Proper cleanup on unmount
   ```

4. **Type Definitions** (`types.ts`)
   ```typescript
   // Core Game Types:
   - Cell: "x" | "o" | null
   - BoardState: Fixed-length array of 9 cells
   - Moves: Valid cell indices (0-8)
   - BoardResult: Win condition with direction/position data
   ```

#### `/contexts` - Global State Management
```
contexts/
└── settings-context.tsx   # Settings state management
```

**Settings Context Analysis:**
- Manages global app settings (difficulty, sounds, haptics)
- AsyncStorage integration for persistence
- Type-safe context with custom hook
- Error handling for storage operations
- Default settings fallback

#### `/config` - Application Configuration
```
config/
└── navigator.tsx          # Navigation configuration
```

**Navigator Analysis:**
- Stack navigator setup with React Navigation
- Type-safe navigation with TypeScript
- Consistent header styling
- Custom font integration in headers

### `/amplify` - AWS Backend Configuration

```
amplify/
├── backend/
│   ├── api/tictactoe/
│   │   ├── schema.graphql     # GraphQL schema definition
│   │   ├── resolvers/         # GraphQL resolvers (VTL)
│   │   └── stacks/           # CloudFormation stacks
│   ├── auth/                  # Authentication configuration
│   └── types/                 # Generated TypeScript types
└── team-provider-info.json    # Environment configuration
```

**Amplify Integration:**
- GraphQL API with basic Todo model (template/foundation)
- Authentication setup (Cognito integration ready)
- Auto-generated TypeScript types
- VTL resolvers for custom business logic
- Environment-specific configurations

## 🧠 Game Logic Deep Dive

### Minimax Algorithm Implementation

The AI opponent uses a sophisticated minimax algorithm with several optimizations:

```typescript
// Minimax Core Logic:
1. Base Cases:
   - Terminal states (win/loss/draw)
   - Maximum depth reached (difficulty control)

2. Recursive Cases:
   - Maximizing player: Choose highest value move
   - Minimizing player: Choose lowest value move

3. Optimizations:
   - Depth penalty for wins/losses (prefer quicker wins)
   - Random selection from equal-value moves
   - Depth limiting for difficulty levels
```

### Difficulty Implementation

```typescript
// Difficulty Levels:
- Beginner (depth 1): Very limited lookahead
- Intermediate (depth 3): Moderate strategy
- Hard (depth 4): Strong tactical play
- Impossible (depth -1): Full game tree search
```

### Board State Management

```typescript
// Board Representation:
- 1D array of 9 cells [0,1,2,3,4,5,6,7,8]
- Maps to 3x3 grid:
  [0][1][2]
  [3][4][5]
  [6][7][8]

// Win Condition Detection:
- Horizontal lines: [0,1,2], [3,4,5], [6,7,8]
- Vertical lines: [0,3,6], [1,4,7], [2,5,8]
- Diagonal lines: [0,4,8], [2,4,6]
```

## 🎨 Styling Architecture

### Style Organization

1. **Component-Level Styles**: Each component has its own `.styles.ts` file
2. **Global Constants**: Colors and shared values in `/utils`
3. **Responsive Design**: Dynamic sizing based on screen dimensions
4. **Theme Consistency**: Consistent color palette throughout

### Color Scheme
```typescript
// Primary Colors:
- Purple tones for backgrounds and primary elements
- Light green for highlights and active states
- Dark purple for text and secondary elements
```

## 🔊 Audio & Haptics System

### Sound Management
- **Preloading Strategy**: All sounds loaded on app initialization
- **Conditional Playback**: Respects user settings
- **Memory Management**: Proper cleanup to prevent memory leaks
- **Sound Files**: 
  - `pop_1.wav` / `pop_2.wav`: Move sounds
  - `win.mp3` / `loss.mp3` / `draw.mp3`: Game end sounds

### Haptic Feedback
- **Light Impact**: For game moves
- **Success Notification**: For wins
- **Error Notification**: For losses
- **Warning Notification**: For draws

## 📱 Platform Considerations

### Cross-Platform Compatibility
- **React Native**: Native performance on iOS and Android
- **Expo**: Simplified development and deployment
- **Web Support**: Browser compatibility via react-native-web

### Performance Optimizations
- **Memoization**: Prevents unnecessary re-renders
- **Efficient State Updates**: Minimal state changes
- **Asset Optimization**: Compressed images and sounds
- **Bundle Splitting**: Code splitting for better load times

## 🚀 Build & Deployment

### Development Workflow
```bash
# Development Commands:
npm start          # Start Expo development server
npm run android    # Run on Android emulator/device
npm run ios        # Run on iOS simulator/device
npm run web        # Run in web browser

# Quality Assurance:
npm run typecheck  # TypeScript type checking
npm run lint       # ESLint code analysis
npm run format     # Prettier code formatting
```

### Production Build Process
1. **Type Checking**: Ensures no TypeScript errors
2. **Linting**: Code quality validation
3. **Testing**: Automated test execution
4. **Bundle Optimization**: Code splitting and minification
5. **Asset Processing**: Image and sound file optimization

## 🔧 Development Tools Configuration

### TypeScript Configuration (`tsconfig.json`)
- **Path Mapping**: Convenient imports with `@` prefixes
- **Strict Mode**: Enhanced type checking
- **Expo Base**: Inherits Expo's optimized TypeScript config

### Babel Configuration (`babel.config.js`)
- **Module Resolution**: Custom path resolution
- **React Native Web**: Web platform support
- **TypeScript Support**: TypeScript compilation

### ESLint Configuration
- **React Hooks**: Hook usage validation
- **TypeScript**: TypeScript-specific linting
- **Prettier Integration**: Automatic formatting

## 🔒 Security Considerations

### Data Security
- **Local Storage**: Sensitive data stored locally only
- **No Network Dependencies**: Core game works offline
- **Input Validation**: Type-safe input handling

### AWS Amplify Security
- **IAM Roles**: Proper permission management
- **API Security**: GraphQL security best practices
- **Authentication Ready**: Cognito integration prepared

## 🧪 Testing Strategy

### Current Testing Setup
- **Jest**: Test runner configuration
- **TypeScript Support**: Type-safe testing
- **React Native Testing**: Platform-specific testing capabilities

### Recommended Test Coverage
1. **Unit Tests**: Game logic functions (board.ts, player.ts)
2. **Component Tests**: UI component behavior
3. **Integration Tests**: Screen navigation and state management
4. **E2E Tests**: Complete user workflows

## 📈 Performance Monitoring

### Key Metrics to Monitor
1. **Bundle Size**: JavaScript bundle optimization
2. **Memory Usage**: Audio and component memory management
3. **Frame Rate**: Smooth animations and interactions
4. **Battery Usage**: Efficient background processing

### Optimization Opportunities
1. **Code Splitting**: Lazy load non-critical components
2. **Image Optimization**: Use WebP format where supported
3. **State Optimization**: Minimize unnecessary re-renders
4. **Background Tasks**: Efficient AI computation

## 🔮 Future Development Roadmap

### Phase 1: Core Enhancements
- **Multiplayer Mode**: Real-time gameplay
- **Enhanced AI**: Multiple AI personalities
- **Statistics**: Detailed game analytics

### Phase 2: Advanced Features
- **Tournaments**: Competitive gameplay modes
- **Social Features**: Friend systems and leaderboards
- **Customization**: Themes and board designs

### Phase 3: Platform Expansion
- **Desktop Apps**: Electron-based desktop versions
- **Progressive Web App**: Enhanced web experience
- **Smart TV**: Apple TV and Android TV support

## 📚 Learning Resources

### Key Technologies
- **React Native**: https://reactnative.dev/
- **Expo**: https://expo.dev/
- **React Navigation**: https://reactnavigation.org/
- **AWS Amplify**: https://aws.amazon.com/amplify/

### Game Development Concepts
- **Minimax Algorithm**: AI decision-making
- **Game State Management**: Efficient state handling
- **Mobile UX**: Touch-based interface design

---

This technical documentation provides a comprehensive understanding of the codebase architecture, design decisions, and implementation details for efficient development and maintenance.
