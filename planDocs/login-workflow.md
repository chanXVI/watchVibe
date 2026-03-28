# Web App Login / Logout / Biometric Auth Workflow

```mermaid
flowchart TD
    Start([Web app launch / page load]) --> CheckSession{Existing session?}
    CheckSession -- Yes --> MainApp[Main app screen]
    CheckSession -- No --> LoginScreen[Login screen]

    LoginScreen --> AuthChoice{Choose auth method}
    AuthChoice --> PasswordLogin[Email/password login]
    AuthChoice --> BiometricLogin[Biometric authentication]
    AuthChoice --> ForgotPassword[Forgot password flow]

    PasswordLogin --> ValidateCredentials[Validate credentials]
    ValidateCredentials -- Success --> LoginSuccess[Login success]
    ValidateCredentials -- Failure --> LoginFailure[Show error]
    LoginFailure --> LoginScreen

    LoginSuccess --> CheckBiometricSetup{Biometric setup enabled?}
    CheckBiometricSetup -- No --> OfferBiometric[Offer biometric enrollment]
    CheckBiometricSetup -- Yes --> MainApp

    OfferBiometric --> EnrollBiometric[Prompt biometric enrollment]
    EnrollBiometric -- Success --> SaveBiometricFlag[Save biometric enabled]
    EnrollBiometric -- Skip --> SkipBiometric[Proceed without biometric]
    SaveBiometricFlag --> MainApp
    SkipBiometric --> MainApp

    BiometricLogin --> BiometricPrompt[Show biometric prompt]
    BiometricPrompt -- Success --> BiometricSuccess[Auth success]
    BiometricPrompt -- Failure --> BiometricFailure[Auth failed]
    BiometricFailure --> LoginScreen
    BiometricSuccess --> MainApp

    MainApp --> Logout[User taps logout]
    Logout --> ClearSession[Clear session & cached auth]
    ClearSession --> LoginScreen

    MainApp --> SessionExpired[Session expired / token invalid]
    SessionExpired --> ClearSession

    %% Additional edge cases
    BiometricPrompt --> BiometricLocked{Too many failures?}
    BiometricLocked -- Yes --> FallbackPassword[Fallback to password login]
    BiometricLocked -- No --> LoginScreen
    FallbackPassword --> LoginScreen

    classDef authStyle fill:#f8f9fa,stroke:#0b5ed7,stroke-width:2px;
    class LoginScreen,PasswordLogin,BiometricLogin,BiometricPrompt,OfferBiometric authStyle;
```

## Notes
- Use secure cookies or secure client-side storage for tokens/refresh tokens and a safe enrollment flag for biometric login.
- In a web-first architecture, handle biometric prompt on the client using WebAuthn or platform-specific browser/mobile support; backend should support token refresh and biometric-enabled login state.
- On successful email/password login, optionally offer biometric enrollment for faster future access.
- Logout should clear session state, revoke tokens, and return the user to the login screen.
- Handle session expiration and biometric lockout by returning to the login screen with a fallback to credentials.
