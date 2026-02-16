# Installation — Blueprint Insight

How to install and enable the Blueprint Insight plugin in your Unreal Engine 5 project.

---

## Requirements

- **Unreal Engine 5.6**
- **Windows** (Editor); Runtime and Commandlets also support Mac and Linux where applicable
- **Optional:** [Data Validation](https://docs.unrealengine.com/5.0/en-US/DataValidation/) plugin (enabled by default)

---

## Steps

1. **Get the plugin**  
   Download Blueprint Insight from the [Fab marketplace](https://www.fab.com) (or your distribution source) and unpack the zip if needed.

2. **Copy into your project**  
   Copy the **`Blueprint_Insight`** folder into your project’s **Plugins** directory:
   ```
   YourProject/
   └── Plugins/
       └── Blueprint_Insight/    ← folder containing Blueprint_Insight.uplugin and Source/
   ```

3. **Load the plugin**  
   - Restart Unreal Editor, or  
   - Build the project so the plugin compiles and loads.

4. **Enable (if needed)**  
   If the plugin is not enabled: **Edit → Plugins**, search for **Blueprint Insight**, check **Enabled**, then restart the editor.

5. **Open the panel**  
   - **Window → Blueprint Insight**, or  
   - In the Content Browser, right‑click a Blueprint asset → **Analyze with Blueprint Insight**.

---

## Verify installation

- **Edit → Plugins** — “Blueprint Insight” should appear under the **Blueprints** category and be enabled.
- **Window** menu — “Blueprint Insight” should be listed.
- Content Browser — Right‑click a Blueprint and confirm “Analyze with Blueprint Insight” and “Add to Investigation Project” appear.

---

## Troubleshooting

- **Plugin doesn’t appear** — Ensure the folder name is exactly `Blueprint_Insight` and that `Blueprint_Insight.uplugin` is inside it. Restart the editor.
- **Build errors** — Confirm you’re on UE 5.6 and that the Data Validation plugin is available (it’s a standard UE plugin). Rebuild from a clean state if needed.
- **Missing menu** — After enabling the plugin, fully restart the editor.

For more help, see [Support](https://github.com/velithstudios/Blueprint_Insight/blob/main/SUPPORT.md) and [GitHub Issues](https://github.com/velithstudios/Blueprint_Insight/issues).
