---
name: ppt-ooxml-translator
description: "AI-agent Skill for PPTX OOXML localization workflows. Use when translating/localizing PPTX by unpacking OOXML, extracting and applying text runs, normalizing terminology, enforcing language-specific fonts, validating XML integrity, and repacking outputs with JSON-friendly interfaces."
---

# PPT OOXML Translator Skill

## Installation

```bash
# from repo root
python3 -m pip install .

# or editable mode
python3 -m pip install -e .
```

## Quick Reference

```bash
# bilingual help
ppt-ooxml-tool help --lang both

# end-to-end one command
ppt-ooxml-tool --json workflow \
  --input ./input.pptx \
  --root ./unpacked \
  --include slides,notes,masters \
  --lang ja \
  --glossary ./examples/glossary.example.json \
  --output ./output.ja.pptx
```

## Input and Output Contract

### Input
- Packed mode: `--input /path/to/file.pptx`
- Unpacked mode: `--root /path/to/unpacked` where root contains:
  - `[Content_Types].xml`
  - `ppt/`

### Output
- Translation table TSV: `<root>/translation.<lang>.tsv`
- Repacked PPTX: `<root>.out.pptx` or explicit `--output`

## Commands

### Help
```bash
ppt-ooxml-tool help --lang zh
ppt-ooxml-tool help --lang en
ppt-ooxml-tool --json help --lang both
```

### Unpack
```bash
ppt-ooxml-tool unpack --input ./input.pptx --output ./unpacked
```

### Collect
```bash
ppt-ooxml-tool collect --root ./unpacked --include slides,notes --output ./translation.ja.tsv
```

### Apply
```bash
ppt-ooxml-tool apply --root ./unpacked --tsv ./translation.ja.tsv
```

### Normalize
```bash
ppt-ooxml-tool normalize --root ./unpacked --include slides,notes,masters --lang ja --glossary ./examples/glossary.example.json
```

### Validate
```bash
ppt-ooxml-tool validate --root ./unpacked --include slides,notes,masters --lang ja
```

### Repack
```bash
ppt-ooxml-tool repack --root ./unpacked --output ./output.ja.pptx
```

### Workflow (single command)
```bash
ppt-ooxml-tool --json workflow \
  --input ./input.pptx \
  --root ./unpacked \
  --include slides,notes,masters \
  --lang ja \
  --glossary ./examples/glossary.example.json \
  --output ./output.ja.pptx
```

### Runfile (JSON job spec)
```bash
ppt-ooxml-tool --json runfile --job ./examples/job.example.json
```

## Language Font Presets
- `en` -> `Calibri`
- `ja` -> `Yu Gothic`
- `zh-cn` -> `Microsoft YaHei`
- `zh-tw` -> `Microsoft JhengHei`
- `ko` -> `Malgun Gothic`
- `ar` -> `Tahoma`

Use `--font` to override preset mapping.

## Glossary Format (Desensitized Example)

```json
{
  "BrandAlpha": "ブランドアルファ",
  "ProductSuiteX": "プロダクトスイートX",
  "FlagshipStore": "旗艦ストア"
}
```

## Integration Notes
- Use `--json` for machine-readable outputs in external clients.
- Keep TSV row count/order unchanged when writing translations.
- Preserve placeholders, variables, and numeric tokens.
- This Skill is model-agnostic. Translation model selection belongs to the caller/client.
