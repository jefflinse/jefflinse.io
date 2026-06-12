+++
title = "Projects"
+++

I write a lot of Go. My side-projects are usually libraries, dev tools, or APIs that scratch an itch or explore an idea. I try to keep them small and focused, albeit not always successfully.

<section class="projects">

<div class="projects-section-label">LLM Orchestration</div>

<div class="projects-stack">

<article class="project-card" data-layer="platform">
  <div class="project-head">
    <span class="project-chip">application</span>
    <h2 class="project-name">Toasters</h2>
    <a class="project-repo" href="https://github.com/jefflinse/toasters" aria-label="Toasters on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">Agentic orchestration platform.</p>
  <p class="project-desc">An LLM orchestration system geared toward getting the most out of smaller, less-capable LLMs running on limited hardware. Supports declarative execution graphs, fault resiliancy, and job management.</p>
  <ul class="project-tags">
    <li>Go</li><li>TUI</li><li>LLM</li><li>Orchestration</li><li>Multi-Provider</li>
  </ul>
</article>

<div class="projects-connector"><span>built on</span></div>

<article class="project-card" data-layer="runtime">
  <div class="project-head">
    <span class="project-chip">library</span>
    <h2 class="project-name">Mycelium</h2>
    <a class="project-repo" href="https://github.com/jefflinse/mycelium" aria-label="Mycelium on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">Go library providing a bounded agent loop with a structured, three-status output contract.</p>
  <p class="project-desc">The conversation loop ends only when the model calls one of three injected terminal tools: <code>complete</code>, <code>request_context</code>, or <code>report_error</code>, so output structure is enforced by tool-call validation rather than JSON-from-prose parsing.</p>
  <ul class="project-tags">
    <li>Go</li><li>LLM</li><li>Provider-Agnostic</li>
  </ul>
</article>

<div class="projects-connector"><span>built on</span></div>

<article class="project-card" data-layer="engine">
  <div class="project-head">
    <span class="project-chip">library</span>
    <h2 class="project-name">Rhizome</h2>
    <a class="project-repo" href="https://github.com/jefflinse/rhizome" aria-label="Rhizome on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">A lightweight, generic graph execution engine in Go.</p>
  <p class="project-desc">Define typed nodes and edges, compile the graph, and run it. Provides conditional branching, loops with cycle protection, and a middleware chain for retries, timeouts, and panic recovery. Opt-in checkpointing snapshots state after every node so a crashed run can resume in a different process, and nodes can pause for human-in-the-loop input via interrupts.</p>
  <ul class="project-tags">
    <li>Go</li><li>Graph Execution</li><li>Zero-Dependency</li>
  </ul>
</article>

</div>

<div class="projects-section-label">Event Sourcing</div>

<div class="projects-stack">

<article class="project-card" data-layer="estoria">
  <div class="project-head">
    <span class="project-chip">library</span>
    <h2 class="project-name">Estoria</h2>
    <a class="project-repo" href="https://github.com/go-estoria/estoria" aria-label="Estoria on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">An event-sourcing toolkit for Go.</p>
  <p class="project-desc">Model application state as a series of state-changing events. Get auditing, replay, and time travel for free. Provides composable primitives: event-sourced aggregates, snapshotting, caching, and lifecycle hooks. Storage is pluggable: choose from Postgres, SQLite, MongoDB, KurrentDB, and S3, or roll your own.</p>
  <ul class="project-tags">
    <li>Go</li><li>Event-Sourcing</li>
  </ul>
</article>

</div>

<div class="projects-section-label">Dev Tools</div>

<div class="projects-stack">

<article class="project-card" data-layer="clic">
  <div class="project-head">
    <span class="project-chip">dev tool</span>
    <h2 class="project-name">clic</h2>
    <a class="project-repo" href="https://github.com/jefflinse/clic" aria-label="clic on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">Define, generate, and run CLI tools from config — or straight from OpenAPI.</p>
  <p class="project-desc">The Command Line Interface Compiler turns a simple YAML/JSON spec — or any OpenAPI 3.x document — into a command-line tool you can run on the fly or compile to a native Go binary. Includes an interactive TUI with response contract-testing and a spec-driven mock server with no server code.</p>
  <ul class="project-tags">
    <li>Go</li><li>CLI</li><li>TUI</li><li>OpenAPI</li><li>Codegen</li>
  </ul>
</article>

</div>

<div class="projects-section-label">Fun / Experimental</div>

<div class="projects-stack">

<article class="project-card" data-layer="standalone">
  <div class="project-head">
    <span class="project-chip">Experiment</span>
    <h2 class="project-name">Symposium</h2>
    <a class="project-repo" href="https://github.com/jefflinse/symposium" aria-label="Symposium on GitHub"><i class="fa-brands fa-github"></i> GitHub</a>
  </div>
  <p class="project-tagline">A tiny CLI that runs an open-ended conversation between two LLMs.</p>
  <p class="project-desc">Each participant is an OpenAI-compatible endpoint with its own model, system prompt, and temperature. History persists in SQLite, and per-session compaction and handoff keep long conversations from blowing past the context window. Socrates and Nietzsche can argue indefinitely.</p>
  <ul class="project-tags">
    <li>Go</li><li>CLI</li><li>LLM</li>
  </ul>
</article>

</div>

</section>
