# TigerStripes
# tiger-id

**Identify tigers from photographs using LLM vision.** Compares two tiger images feature-by-feature (eye-patch shape, whisker pads, stripe spacing, mid-body stripe cluster) and decides whether they show the same individual.

> Tiger stripes are fingerprints. No two tigers share the same pattern. This tool asks an LLM to do that comparison for you.

## How to run (4 steps)

1. **Clone & install:** `git clone <this-repo> && cd tiger-id && pip install -r requirements.txt`
1. **Set your API key:** `export ANTHROPIC_API_KEY=sk-ant-...` (get one at console.anthropic.com)
1. **Run on two photos:** `python -m tiger_id.compare your_sighting.jpg reference.jpg --name "Bufferwaali"`
1. **Read the verdict:** The script prints a feature-by-feature checklist and a final “same tiger?” call with confidence.

## Example

```bash
python -m tiger_id.compare bandhavgarh_morning.jpeg reference_bufferwaali.jpeg --name "Bufferwaali"
```

```
=== Tiger ID Comparison ===

👁  Face
  ✅ Eye Patch Shape              MATCH
  ✅ Nose And Whisker Pad         MATCH
  ✅ Forehead Stripes             MATCH

🐯 Body (critical)
  ✅ Mid Body Stripe Cluster      MATCH
  ✅ Stripe Spacing And Curvature MATCH
  ✅ Conflicting Stripe Breaks    NONE

📐 Build & posture
  • Frame                         Lean, athletic adult tigress
  • Gait Or Posture               Resting; same shoulder structure
  • Expression                    Alert, calm

🎯 Conclusion: SAME TIGER  (confidence: HIGH)
   Identified as: Bufferwaali
   Reasoning: Stripe pattern across the flank matches in spacing,
   curvature, and break points. Facial markings also align.
```

## How it works

The script sends both images to Claude with a structured prompt asking for JSON output. The prompt instructs the model to mark features as MATCH, MISMATCH, or UNCLEAR — never to guess — and to only return `same_tiger: true` when enough features clearly agree.

The model used by default is `claude-opus-4-5`. You can swap it via `--model claude-sonnet-4-5` for a faster, cheaper run. To switch to OpenAI’s GPT-4o or Google’s Gemini, replace the `anthropic` client call in `tiger_id/compare.py` with the equivalent SDK call — the prompt and image-encoding logic stay identical.

## Why this matters

Manual stripe-matching needs a trained eye, a stripe database, and time. India has roughly 3,700 tigers across 50+ reserves; every park visitor with a phone is a potential data point. If a tourist can verify a sighting in 30 seconds, conservation gains a passive census layer that didn’t exist before.

This repo is a proof of concept, not a production system. For real conservation work, integrate it with [WildBook](https://www.wildbook.org/) or [ExtractCompare](https://www.conservationresearch.org.uk/Home/ExtractCompare/), which use computer-vision pipelines purpose-built for striped-animal ID.

## Flags

|Flag     |Default          |Notes                                       |
|---------|-----------------|--------------------------------------------|
|`--name` |none             |Name of the reference tiger (cosmetic only) |
|`--model`|`claude-opus-4-5`|Any Anthropic vision model                  |
|`--json` |off              |Print raw JSON instead of the formatted view|

## License

MIT. See `LICENSE`.

## Credits

Inspired by a sighting in Bandhavgarh, India — and a tourist’s question about whether the tigress in his photo was the same one already named on social media.
