# .NET MAUI Login / Logout / Biometric Auth Workflow

```mermaid
flowchart TD
    Start([App launch]) --> CheckSession{Existing session?}
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
- Use `SecureStorage` for tokens/refresh tokens and a secure flag for biometric enrollment.
- In .NET MAUI, use platform biometric APIs or a cross-platform plugin like `Plugin.Fingerprint` / `Community Toolkit` for biometric prompt.
- On successful email/password login, optionally enroll biometrics to simplify future logins.
- Logout should clear session state and remove any local auth tokens.
- Handle session expiration and biometric lockout by returning to the login screen with fallback to credentials.
