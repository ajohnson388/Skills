```
struct MicrophoneEnvironmentKey: EnvironmentKey {
    static let defaultValue: Microphone = Microphone()
}

extension EnvironmentValues {
    var microphone: Microphone {
        get { self[MicrophoneEnvironmentKey.self] }
        set { self[MicrophoneEnvironmentKey.self] = newValue }
    }
}
```
