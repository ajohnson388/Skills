```
@Observable
public final class Microphone {

    public var isEnabled = false

    private let audioPermission: AudioPermission
    private let urlHandler: URLHandler

    public init(
        audioPermission: AudioPermission = AVAudioApplication.shared,
        urlHandler: URLHandler = UIApplication.shared
    ) {
        self.audioPermission = audioPermission
        self.urlHandler = urlHandler
        self.isEnabled = audioPermission.recordPermission == .granted
    }

    public func enable() {
        switch (audioPermission.recordPermission) {
        case .granted:
            isEnabled = true
        case .denied:
            if let settingsUrl = URL(string: UIApplication.openSettingsURLString) {
                urlHandler.openURL(settingsUrl)
            }
        default:
            audioPermission.requestRecordPermission { [weak self] granted in
                self?.isEnabled = granted
            }
        }
    }
}
```
