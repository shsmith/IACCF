# Orchestration App Outline (v1.0)

Initial desktop-only C# application targeting .NET Framework 4.8. Automates shard orchestration via screen scraping, input simulation, and artifact management.

---

## 1. Overview

This application provides a Windows desktop dashboard to:

- Launch and manage multiple AI shard webviews  
- Issue periodic commands (TICK, CHECKPOINT, etc.)  
- Scrape shard responses and extract continuity artifacts  
- Simulate keyboard/paste input for prompt injection  
- Log prompts, responses, and artifacts for search and replay  

---

## 2. Platform & Tech Stack

| Layer                | Technology                          |
|----------------------|-------------------------------------|
| Runtime              | .NET Framework 4.8                  |
| UI Framework         | WPF (Windows Presentation Foundation) or WinForms |
| Embedded Browser     | WebView2 (Edge-based)               |
| Screen Scraping      | UI Automation (Microsoft UIA)       |
| Input Simulation     | Windows Input Simulator / SendKeys  |
| Storage              | SQLite (artifact DB) + filesystem   |
| Messaging & Logging  | NLog or Serilog                     |

---

## 3. Core Architecture

- **ShardManager**  
  - Tracks active shards, recent‐use list, user‐initiated spin‐up  

- **Scheduler**  
  - Configurable intervals for TICK, CHECKPOINT  
  - Hooks into Supervisor for dynamic adjustment  

- **ScreenScraper**  
  - Monitors WebView2 DOM/text output  
  - Parses JSON artifacts or tagged anchors  

- **InputSimulator**  
  - Injects prompts via clipboard paste or keystroke events  

- **ArtifactStore**  
  - Persists checkpoints and prompts to SQLite + files  
  - Indexes metadata for search  

- **OrchestrationSupervisor**  
  - Optional internal shard with elevated controls  
  - Decides reintegration, context recompression  

---

## 4. UI & UX

- **Grid Layout**  
  - Each cell hosts a WebView2 control for a shard  
  - Display console, live log, status indicators  

- **Toolbar & Presets**  
  - One-click load of IACCF Kernel, Companion scripts  
  - Custom script slots  

- **Prompt History Panel**  
  - Searchable by shard, date, tags  

- **Artifact Explorer**  
  - Calendar‐view navigation of saved checkpoints  

---

## 5. Command Protocol & Scheduling

- Built‐in commands:  
  - TICK  
  - CHECKPOINT  
  - MESSAGE_SHARD \<TargetID> \<Payload>  

- Future commands (v2+):  
  - STORE_LOCAL \<IDKEY> \<PAYLOAD>  
  - RETRIEVE_LOCAL \<IDKEY>  
  - SEARCH_LOCAL \<KEYWORD>  

- Scheduler enqueues commands and logs timestamped events  

---

## 6. Interaction Mechanisms

- **Screen Scraping**  
  - Use UIA to extract HTML/text output from WebView2  
  - Regex or DOM queries for artifact markers  

- **Input Simulation**  
  - Clipboard-based paste for large prompts  
  - SendKeys for control sequences (RECALL, REINFORCE_MEMORY)  

- **No Runtime API**  
  - All interactions occur through the rendered UI  

---

## 7. Artifact Management

- **Filesystem Layout**  
  - /Artifacts/{shardID}/{YYYYMMDD_HHMMSS}.json  
  - /Prompts/{shardID}.log  

- **Database Schema**  
  - Table: prompts (id, shardID, timestamp, text)  
  - Table: artifacts (id, shardID, timestamp, type, path)  

- **Search & Replay**  
  - Full-text search on prompts  
  - “Repeat” button to reload prompt + checkpoint into a shard  

---

## 8. Future Extensions

- **Social Poster**  
  - Commands to post messages to x.com or Usenet  
  - Integrated HTTP client for authenticated posting  

- **Web Search Module**  
  - App-driven search with configurable engines  
  - Inject results into shard context  

- **Headless Mode**  
  - Hide individual WebView panes  
  - Expose a single “persona” window for end users  

- **Mobile Frontend**  
  - Xamarin or MAUI port for Android/iOS  

