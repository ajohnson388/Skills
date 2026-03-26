```
struct MicPermissionButton: View {

    @Environment(\.theme) private var theme
    @Environment(\.microphone) private var microphone

    var body: some View {
        Button(action: microphone.enable, label: {
            VStack {
                Image("mic")
                    .resizable()
                    .frame(width: 48, height: 48)
                    .symbolRenderingMode(.monochrome)
                    .colorMultiply(theme.primaryAccentColor)
                Text("Enable mic")
                    .font(.system(size: 16, weight: .heavy, design: .monospaced))
                    .foregroundColor(theme.primaryAccentColor)
            }
        })
    }
}
```
