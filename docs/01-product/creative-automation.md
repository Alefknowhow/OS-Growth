# Creative Automation

Goal: when a creative is requested (by the operator, by voice, or by an agent after a fatigue signal), Growth OS produces ready-to-review creatives from the client's media and context, using pre-built skills and connected tools.

## Flow
```
Request (command / signal / experiment / client request)
  → Creative Strategist: angle, hook options, format, number of variants (uses profile, performance, learnings)
  → Creative Brief (structured)
  → Skill selection (format → skill, e.g. ugc-cut-reels, motion-offer, static-offer)
  → Creative Producer runs the skill:
        select assets from Media Library (rights checked)
        Content Agent writes copy/script/captions
        Higgsfield MCP for generated images/video/b-roll/animation
        Motion graphics (Remotion) for edits, captions, logo, end card, composition
        render variants to placement specs
  → Creative QA: specs, brand rules, prohibited claims, legibility, safe zones, consent
  → Internal review (operator) → optional Client review (portal)
  → Publish: Plan "create ads with creatives X, Y in ad set Z" → Policy Engine → Execution
  → Performance tracked per creative → learnings (winning/losing angles, hooks)
```

## Request examples
- "3 vídeos reels para a oferta de clareamento, usando os depoimentos de setembro."
- "Uma versão estática 4:5 do criativo campeão com a nova promoção."
- Automatic: fatigue signal (frequency ↑, CTR ↓) on the top ad → Strategist proposes refresh → job runs → review.

## Initial formats
Static feed (1:1, 4:5), static stories (9:16), carousel, UGC/testimonial cut (9:16 with captions), motion offer (animated promo from logo + photos + copy), AI b-roll reels (client footage + generated b-roll), product/photo animation (image-to-video). Details per format in `docs/03-ai/creative-skills.md`.

## Variants
Each job produces N variants across a controlled dimension (hook, first 3 seconds, headline, visual) so they can run as an experiment.

## Cost control
Each job has an estimated and actual cost (generation credits, render time, model tokens). Per-client monthly creative budget; jobs above the per-job limit require approval.

## Review
- Internal review shows variants side-by-side, brief, QA report, assets used and estimated cost.
- Reviewer can approve, reject with reason, or request changes in natural language (creates a revision job).
- Client review via portal when the client's settings require it.

## Learning loop
Published creatives link to ads; performance (hook rate, CTR, CPA, spend) rolls up to creative, angle and hook. Concluded tests write learnings used by the Strategist next time.
