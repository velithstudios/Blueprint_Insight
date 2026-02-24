---
title: Support
layout: default
nav_order: 5
---

# Support

Contact, FAQ, and troubleshooting for Blueprint Insight.

---

## Contact

- **Documentation & support** — This site and the [Blueprint Insight GitHub repo](https://github.com/velithstudios/Blueprint_Insight) are the official sources. Use the in-editor **Help** tab for quick reference.
- **Discord** — [Join the community server](https://discord.gg/XPGJf7V9) for support, bug reports, and feature requests.
- **Plugin purchase** — [Fab](https://www.fab.com/) for purchase and download.

---

## Frequently asked questions

### How do I fix circular dependencies?

Go to the **Project Dashboard** and click **Visualize Chain** on the Circular Loops card. Identify the assets in the cycle. The usual fix is to introduce a Blueprint Interface or Event Dispatcher to break the hard reference chain.

### Why is the analysis score low?

The score is based on logic complexity, memory footprint, and maintainability. Open the **Asset Inspector** for the Blueprint and check the listed issues and suggestions.

### Does the plugin modify my assets?

No. The plugin is a static analysis tool. It reads graph structure but does not write or save changes to your assets unless you explicitly use an action such as an **Auto-Fix** (e.g. Decouple with Interface, Nativize).

### The plugin won’t load. What should I check?

- Ensure the plugin is in `YourProject/Plugins/Blueprint_Insight`.
- Ensure you’re on **Unreal Engine 5.5 or 5.6** (or a compatible 5.x version).
- Build the project or restart the editor so the plugin compiles.
- Enable the **Data Validation** plugin if the plugin descriptor requires it.
- Check the Output Log for errors when the editor starts.

### AI / Chat isn’t responding

- In **Settings**, confirm the **AI provider** and **Endpoint** (and **API key** for cloud providers) are set correctly.
- For **Ollama**, ensure Ollama is running (e.g. `http://127.0.0.1:11434`) and the model in Settings is installed.
- Check the Output Log for HTTP or API errors.

### Runtime Debugger shows no data

- Ensure you’ve started **Play in Editor** (PIE).
- If you use the in-game Gameplay Debugger, press the key shown in the category (e.g. Numpad 5).
- In **Settings**, check that runtime / phantom debugger options are enabled if required.

---

## Troubleshooting

| Issue | Suggestion |
|-------|------------|
| Scan is slow on a large project | First scan can take a minute or more. Use **Scheduled Scan** with a longer interval, or run scans manually when needed. |
| Dependency graph is empty | Select a single Blueprint asset and open the **Dependency Graph** tab in the Inspector. |
| Export (UML/Markdown) not found | Exports go to `Saved/BlueprintInsight/`. Use the **History** tab and **Open Folder** to locate them. |
| Commandlet fails in CI | Use the **Audit** commandlet for project scan; it exits with code 1 when critical issues exist. For architecture, use the **Architecture** commandlet with `-Schema=...`. |

---

## License and redistribution

**Copyright Velith Studios.**  
License terms are provided with your purchase or distribution (e.g. Fab Personal/Professional). Do not redistribute the plugin except as permitted by that license.
