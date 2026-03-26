```
@Observable
public final class Router {
    public var screenBlock: ScreenBlock?
    public var currentModal: Modal?
    public var currentTab: AppSection = .metronome
    public var topBarHeight = 80

    public func open(deepLink: DeepLink) {
        switch deepLink {
        case .openTuner:
            currentTab = .tuner
        }
    }

    @ViewBuilder
    public func contentView(for appSection: AppSection) -> some View {
        switch appSection {
        case .metronome:
            MetronomeContentView()
                .tag(AppSection.metronome.position)
        case .tuner:
            TunerContentView()
                .tag(AppSection.tuner.position)
        case .scale:
            FretboardContentView()
                .tag(AppSection.scale.position)
        case .midi:
            MIDIContentView()
                .tag(AppSection.midi.position)
        }
    }
}
```
