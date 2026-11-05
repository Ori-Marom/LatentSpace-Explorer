# Setup and troubleshooting

## Java and JavaFX

The application combines Swing with JavaFX through `JFXPanel`. Both the compiler and runtime
need the JavaFX modules `javafx.controls` and `javafx.swing`.

The existing launcher references JavaFX SDK **23.0.2**. JavaFX 23 requires **JDK 21 or later**;
see the [official release notes](https://openjfx.io/highlights/23/). If using another JavaFX
release, check its own JDK requirement. Use an SDK matching the operating system and CPU.

Check your tools:

```sh
java -version
javac -version
```

## Windows: PowerShell

Run from the repository root and supply your installed SDK's `lib` path:

```powershell
$javafx = 'C:\path\to\javafx-sdk-23.0.2\lib'
New-Item -ItemType Directory -Force bin | Out-Null
$sources = Get-ChildItem src -Recurse -Filter *.java | ForEach-Object FullName
javac --module-path $javafx --add-modules javafx.controls,javafx.swing -d bin $sources
if ($LASTEXITCODE -eq 0) {
    java --module-path $javafx --add-modules javafx.controls,javafx.swing -cp bin ui.MainFrame
}
```

The existing `run.bat` sets its own fixed JavaFX path. Setting an environment variable before
running it will not override that assignment. The commands above let you supply the correct
path without editing the launcher.

## macOS/Linux: shell

Use the SDK built for your platform. From the repository root:

```sh
JAVAFX_PATH='/path/to/javafx-sdk-23.0.2/lib'
mkdir -p bin
javac --module-path "$JAVAFX_PATH" --add-modules javafx.controls,javafx.swing \
  -d bin src/model/*.java src/math/*.java src/command/*.java src/ui/*.java && \
java --module-path "$JAVAFX_PATH" --add-modules javafx.controls,javafx.swing \
  -cp bin ui.MainFrame
```

These instructions are derived from the existing launcher; this documentation update does
not claim that builds or graphical behavior were tested on each platform.

## Optional: regenerate embeddings

The bundled JSON files let you start without running Python. To regenerate them, use a
Python environment with the dependencies imported by the existing generator:

```sh
python -m pip install gensim scikit-learn numpy
python embeddings_gen.py
```

Generation downloads `glove-wiki-gigaword-100` through Gensim, selects 5,000 words, reduces
their 100-dimensional vectors to 50 PCA components, and writes both JSON files to the current
directory. It requires internet access for an uncached model. It replaces existing JSON files,
so keep a copy first if you want to retain the bundled data. No dependency versions are pinned
in the supplied project.

## Common issues

**`Module javafx.controls not found`** — Check that the module path points to the SDK's `lib`
folder, not its parent folder, and that the JAR files exist there.

**Unsupported class version / module cannot be read** — Check that the JDK is compatible with
the chosen JavaFX SDK and that `java` and `javac` refer to the intended installation.

**Missing JSON files triggers Python** — The loader looks for `full_vectors.json` and
`pca_vectors.json` relative to the current working directory. Launch from the repository root.
If data is genuinely absent, the loader attempts `python`, then `py`, then `python3`.

**Word not found** — Use a word from the loaded list. The vocabulary is a subset of the model,
and some lookups are case-sensitive; lowercase inputs are a useful starting point.

**Analogy result differs from an example** — The ranking depends on the embeddings and
available vocabulary. Example queries are demonstrations, not expected test outputs.
