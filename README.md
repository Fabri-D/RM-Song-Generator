# RM Song Generator: Algorithmic Music Composition System

🇪🇸 This README is also available in [Spanish](README_es.md).

**RM Song Generator** is a modular algorithmic system for music composition. It uses advanced music theory, combinatorics, symbolic logic, and data science principles to generate unique, structured, and reproducible pieces. Unlike neural network-based models, it relies on a **deterministic rule-based architecture** with explicit segmentation and a hierarchical decision system.

---

## 🔧 Core Architecture

RM Song Generator does **not** use templates or supervised learning. Instead, it is built around:

- A **database of over 1,000 scales and modes**, including their harmonic degrees.
- A **harmonic analysis engine** that performs over **1 million precomputed comparisons** between scales to determine compatible modulations, including transition chords and similarity scores.
- A **rhythmic generation module** that selects time signatures and tempo per section and generates internal durations using **combinatorial subdivisions** (eighth notes, triplets, sextuplets, etc.).
- A **melody selection engine** that probabilistically chooses from four scale sources per chord: current mode, relative mode, foreign scale, or hybrid combination. It outputs structured melodic plans: dyads, triads, pentatonics, fugues, and more.
- An **arpeggio and solo engine** capable of generating dynamic accompaniments and solo lines based on parametric patterns, direction, repetition logic, and degree expansion.
- A **harmonic memory system** that stores previously generated sections and reintroduces them through justified modulations using bridge scales from a modal compatibility graph.

Each section is independently built with its own tempo, meter, harmonic content, and melodic logic. Final output is rendered in `.mp3` using seven fixed instruments: **double bass, cello, viola, low/high violin, clarinet, and harp**.

---

## 🌀 Generation Pipeline

### 1. Initial Rhythm and Modal Recognition

- Begins with random selection of meter (e.g. 3/4, 5/4, 6/8) and tempo (20–300 BPM).
- Generates a pseudo-random sequence of chords (2–4 notes) with durations that sum up to complete the section.
- Identifies a likely **reference chord** (often the longest), and infers **mode and tonic** by weighted interval matching against all known modes.
  - Root match = full weight
  - 3rd, 5th, 7th, 9th, 11th, 13th = decreasing weights
- The best-fitting mode above a threshold is selected as the **harmonic base**.

### 2. Tonal Reordering and Progression Structuring

- Once tonic and mode are set, chords are reordered for **maximum harmonic continuity**.
  - Pre-tonic chords are sorted by forward similarity
  - Post-tonic chords by reverse similarity
- A jazz progression library (200+ idioms, such as II–V–I or III–VI–II–V) is optionally used to validate the harmonic flow. If no idiom fits or if idiomatic projection is disabled (by random valve), a free-form modulation is generated.
Throughout this process, the tonic chord tends to act as a harmonic anchor: it seeks structural prominence by aligning with moments of rest or stabilization within the piece, helping organize the composition even in non-idiomatic or exploratory sections.

### 3. Arpeggios and Harmonic Expansion

- Some chords are transformed into **arpeggios**.
- This involves:
  - Subgrouping notes (1–3–5, or more)
  - Applying mirrored, looped, or expanded structures
  - Adding **color tones** (7th–13th)
- Rhythmic durations are assigned proportionally using note values, tuplets (including triplets and quintuplets), and final adjustment to ensure accurate fit within the measure. While the system does not implement interpretative swing, it generates rhythmically varied and musically coherent lines using probabilistic placement of durations.

### 4. Melody and Solo Design

- Melody generation adapts to context: **mode, tonic, tempo, meter, and progression.**
- Scale choice is probabilistic:
  - Current mode
  - Relative mode
  - External scale
- Melodic phrases use structure generators: dyads, triads, fugues, cycles, or pentatonics. Rhythmic variation, silence, and motif development are supported.
- **Clarinet** is default melodic voice. **Harp** may take over for contrast.

### 5. Harp Solos and Fugue Structures

- Generated as expressive, recursive structures based on degrees and rhythm:
  - Expanded arpeggios
  - 2-note fugues
  - Motif alternations (e.g. 1–5–3 alternating with higher tones)
  - Descending responses after ascent
- Rhythmic patterns may include tuplets, irregular groupings, and syncopated motifs, but do not currently support full polyrhythmic layering or expressive dynamics like swing or planned silence distribution.

### 6. Memory and Modulation Between Sections

- Full sections (chords, melodies, tempo, mode, tonic) are stored.
- Reuse is handled through:
  - **Modal distance evaluation** (from compatibility graph)
  - **Bridge scale detection**
  - Generation of a full **intermediate passage**
  - Reexposure of previous section


### 7. Orchestration and Audio Rendering

- Each instrument receives notes based on **range constraints** and **voice leading** rules.
- Arpeggios are distributed across strings; harp plays fast virtuosic lines.
- Expressiveness is encoded via:
  - CC11 (expression), velocity, dynamics curves.
- MIDI events are rendered with **FluidSynth** and **custom soundfonts**, exported to `.mp3`.

---

## 🎯 Contributions and Positioning

RM Song Generator offers a **transparent alternative** to neural nets:

| Feature                     | Neural Nets                     | RM Song Generator              |
|----------------------------|----------------------------------|-------------------------------|
| Transparency               | Black-box                        | Fully explainable             |
| Reproducibility            | Not guaranteed                   | Guaranteed                    |
| Control                    | Limited                          | Full parameter control        |
| Harmonic Depth             | Learned statistically            | Built on formal theory        |
| Modulation Handling        | Implicit or absent               | Explicit with bridge scales   |
| Theoretical Adaptability   | Needs retraining                 | Extendable via modal logic    |

RM combines **algorithmic rigor** and **musical awareness**, contributing to:

- Deterministic tonal logic with progressive validation
- 1M+ precomputed scale comparisons for modulation
- Hybridization of idiomatic progressions and free generation
- Modular, traceable, extensible architecture
- Form coherence via memory and section reintroduction
- Reproducible output in `.mp3` with expressive orchestration

---

## 🔬 Applications and Future Directions

### 🎨 Creative Use

- Composition (film, games, theater)
- Experimental sound exploration
- Accompaniment engine

### 📚 Educational Use

- Harmony, orchestration, modal theory
- Visualizing modal transitions and scale logic

### 🔭 Research Use

- Comparisons between generative strategies
- Modal modulation analysis
- Integration with **symbolic AI** (planning, expert systems)
- Real-time adaptation and perception modeling

---

## ⚠️ Limitations (Current Scope)

- Only supports **7-note scales** (no pentatonics, octatonics)
- Fixed meter per section (no internal meter changes)
- No feedback loop from audio perception or preference learning
- MIDI/audio engine limits realism of human articulation
- The modulation engine currently supports over 1,000 modes and performs more than one million pairwise comparisons efficiently through precomputed similarity matrices. While this design scales well within the current harmonic dataset, future expansions to several thousand modes may benefit from index-based optimizations to maintain performance.
- While the modulation engine supports over 1,000 modes and millions of pairwise comparisons, future scalability with larger modal datasets may benefit from index-based optimizations.
The current implementation spans multiple files, with a centralized core script orchestrating most of the harmonic, rhythmic, and melodic logic in an integrated pipeline. While harmony, rhythm, and melody are not fully separated as independent modules, the codebase follows a structured logic that facilitates updates, experimentation, and feature extension. Additional decoupling could support future modularization and hot-swapping of subsystems for real-time control or alternative strategies.

---

## 🌐 Website and Access

**🔗 https://reharmonizationmaps.com**  
Explore the interactive heatmap, analyze scale intersections, and generate full songs.

---

## 🧠 Closing Note

RM Song Generator is **not** designed to simulate a human composer—it showcases what a **knowledgeable algorithm** can create with explicit music theory, tonal memory, structural awareness, and internal logic. It contributes to a field of **computational creativity** where clarity, structure, and emotion can coexist.

▶️ [Download demo video](videosonggenerator.webm)

