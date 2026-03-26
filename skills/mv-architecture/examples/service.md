```
public final class NotePlayer {

    private static let lowBasePitch = Pitch(note: .e, octave: 2)
    private static let highBasePitch = Pitch(note: .e, octave: 4)

    private let engine = AVAudioEngine()
    private let highBaseNoteAudioPlayer = StringNoteAudioPool(
        pitch: highBasePitch,
        fileName: "E4"
    )
    private let lowBaseNoteAudioPlayer = StringNoteAudioPool(
        pitch: lowBasePitch,
        fileName: "E2"
    )

    public init() {
        do {
            highBaseNoteAudioPlayer.setup(for: engine)
            lowBaseNoteAudioPlayer.setup(for: engine)
            try engine.start()
        } catch {
            print("Failed to setup note audio player: \(error)")
        }
    }

    public func play(_ pitch: Pitch) {
        if pitch.semitoneIndex <= Self.lowBasePitch.semitoneIndex {
            lowBaseNoteAudioPlayer.play(pitch)
        } else {
            highBaseNoteAudioPlayer.play(pitch)
        }
    }

    public func stop() {
        lowBaseNoteAudioPlayer.stop()
        highBaseNoteAudioPlayer.stop()
    }
}
```
