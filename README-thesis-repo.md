# Big Sister: Bachelor Thesis 

A rule-based digital forensics file-analysis tool, built as part of a bachelor's
thesis at Maastricht University comparing AI-assisted, tool-assisted, and
unaided approaches to CTF forensics challenges. This tool represents the
"tool-assisted" condition used in the empirical study.

It began as a fork of [Big-Sister](https://github.com/IlikeEndermen/Big-Sister), a file-analysis
tool built with a team (Irina, Mada, Stanimira) for [team project context —
e.g. a CTF/course]. This repo is a solo continuation that replaces the
original approach with a YAML-driven rule engine.

## What it does

Given an input file, the tool runs a configurable set of analysis rules
against it (e.g. metadata extraction, steganography checks, file-type
inspection) and reports which rules matched, so a user can quickly see what's
worth investigating further.

## Design

- **YAML-driven rule engine** — rules are defined declaratively in YAML, not
  hardcoded, so new checks can be added without touching the core logic.
  Rule values are loaded with `yaml.safe_load` rather than a general parser,
  and conditions go through a structured parser rather than `eval`, so a rule
  file can't execute arbitrary code.
- **Priority sorting** — rules run in a defined order via priority fields,
  using `dataclasses` for the rule/condition models.
- **Safe subprocess execution** — external tools (e.g. `exiftool`, `binwalk`,
  `foremost`) are invoked with argument lists rather than `shell=True`, with
  timeouts, and the tool skips a rule gracefully if its underlying binary
  isn't installed rather than crashing.

## Requirements

- Python 3.[x]
- External tools used by the rules: `exiftool`, `binwalk`, `foremost`
  [confirm full list against your rule YAML files]
- `pip install -r requirements.txt`

## Usage

```bash
python [entry_point.py] <path-to-file> [--rules rules.yaml]
```

[Replace with the actual CLI invocation once confirmed.]

## Running with Docker

```bash
docker build -t thesis-file-analysis .
docker run --rm -v $(pwd)/samples:/samples thesis-file-analysis /samples/<file>
```

> **Note:** the Dockerfile currently references a `src/steganography/`
> directory that no longer exists and doesn't install `foremost`. Fix both
> before relying on this.

## Tests

Not yet included — `evaluate_condition` and `_parse_rule` are pure functions
and good first candidates for a `pytest` suite.

## Thesis context

Written as part of a bachelor's thesis (Maastricht University, DACS)
evaluating how AI assistance, this kind of purpose-built tool, and no tooling
at all compare when solving CTF forensics challenges. See the accompanying
thesis for the full study design and results.

## Credits

Originally based on [Big-Sister](https://github.com/IlikeEndermen/Big-Sister), built with Irina, Mada, and
Stanimira. `file_handler.py` and `terminal.py` carry over from that project;
the rule engine and YAML system are new for this thesis.
