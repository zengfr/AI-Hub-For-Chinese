---
title: Technical Stack
description: "Comprehensive overview of the technology stack, development tools, and architectural decisions."
inclusion: always
---

# Wasteland Tarot - Technical Stack

## Frontend Architecture

### Core Framework
- **Next.js 15.1.7**: React-based full-stack framework with App Router
- **React 19**: Latest React with concurrent features
- **TypeScript**: Full type safety across the application
- **Tailwind CSS**: Utility-first styling framework

### UI Components
- **Custom Components**: Tailored UI components in `/src/components/ui/`
- **Layout Components**: Header, Footer, Background effects
- **Form Components**: Authentication forms with validation
- **Responsive Design**: Mobile-first approach

### State Management & Data Fetching
- **API Integration**: Custom API client in `/src/lib/api.ts`
- **JWT Authentication**: Token-based authentication system
- **Local Storage**: Client-side session persistence

## Backend Architecture

### API Framework
- **FastAPI**: Modern Python web framework for APIs
- **SQLAlchemy**: ORM for database operations
- **SQLite**: Lightweight database for development
- **JWT**: JSON Web Token authentication

### Database Schema
- **Users**: Authentication and profile data
- **Cards**: Tarot card definitions and meanings
- **Readings**: User reading history and spreads

## Development Tools

### Build & Development
- **Bun**: Fast JavaScript runtime and package manager
- **Next.js Dev Server**: Hot reload development environment
- **TypeScript Compiler**: Type checking and compilation

### Testing Framework
- **Cypress**: End-to-end testing suite
- **Playwright**: Browser automation and accessibility testing
- **Custom Scripts**: Accessibility and visual testing automation

### Code Quality
- **ESLint**: JavaScript/TypeScript linting
- **Prettier**: Code formatting
- **TypeScript Strict Mode**: Enhanced type safety

## Deployment & Build

### Build Process
```bash
bun run build    # Production build
bun run dev      # Development server
bun run test     # Test suite execution
```

### Environment Configuration
- **Development**: Local SQLite database
- **Production**: Configurable database connection
- **Environment Variables**: Secure API endpoints and secrets

## Performance Considerations

### Frontend Optimization
- **Code Splitting**: Automatic route-based splitting
- **Image Optimization**: Next.js built-in image optimization
- **CSS Optimization**: Tailwind CSS purging and minification
- **Bundle Analysis**: Size monitoring and optimization

### Backend Performance
- **Database Indexing**: Optimized queries for card and user data
- **API Response Caching**: Strategic caching for static data
- **Connection Pooling**: Efficient database connection management

## Security Features

### Authentication & Authorization
- **JWT Tokens**: Secure stateless authentication
- **Password Hashing**: Secure user credential storage
- **Protected Routes**: Client and server-side route protection

### Data Security
- **Input Validation**: Comprehensive request validation
- **SQL Injection Protection**: ORM-based query protection
- **CORS Configuration**: Controlled cross-origin requests

## Development Workflow

### File Structure Standards
- **App Router**: Page components in `/src/app/`
- **Reusable Components**: Shared components in `/src/components/`
- **Utilities**: Helper functions in `/src/lib/`
- **Types**: TypeScript definitions in appropriate modules

### API Integration Patterns
- **Centralized API Client**: Single source for all API calls
- **Error Handling**: Consistent error management across requests
- **Loading States**: Proper UX for async operations

## Browser Compatibility
- **Modern Browsers**: Chrome, Firefox, Safari, Edge (latest versions)
- **Mobile Support**: iOS Safari, Chrome Mobile
- **Progressive Enhancement**: Graceful degradation for older browsers

## Internationalization
- **Chinese Localization**: Full UI translation support
- **RTL Support**: Right-to-left text direction ready
- **Date/Time Formatting**: Locale-specific formatting