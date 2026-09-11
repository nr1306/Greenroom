# Greenroom — 5-minute pitch speech

*~720 words. Read at a normal pace, this runs about 5 minutes.*

---

## 1. The problem (~45 seconds)

Think about any live event. A conference, a demo day, a church service, a school
show. Someone is sitting at a desk in the back of the room. They have a paper cue
sheet, and they are the only person who knows what happens next.

Lights up. Bring in the speaker. Put the slides on screen. Move to the next thing.

That whole show runs on one person's memory.

And here is the part that breaks. You rehearse it once, it goes perfectly. Then
the next speaker walks on, and you do the same sequence again — but nothing checks
whether the room is actually ready this time. If that speaker's slides never got
uploaded, the operator still hits the button. The screen goes blank in front of
the audience.

## 2. What Greenroom is (~45 seconds)

Greenroom is a live-show director.

It watches one rehearsal, learns the cue sequence, and then reuses that same
sequence for the next speaker.

Two rules make it safe.

One: the operator has to approve the exact plan before anything happens. Not "a"
plan — that exact plan, matched by a hash.

Two: before every single stage change, the backend checks the conditions right
now. Not what was true at rehearsal. Right now.

One segment, three scenes: introduction, presentation, holding.

## 3. The three speakers (~2 minutes 30 seconds)

Let me show you with three people.

**Maya.** Maya's rehearsal already ran, live, and it produced two things. Three
receipts — a signed record of every cue that actually committed. And a reusable
procedure, stored in a tool called Rote, which records and replays procedures.
That is the thing we reuse.

**Ravi.** Different speaker, same show. We pick Ravi, we leave the production
notes alone, and we create a live run.

Behind that one click, four things happen.

Cognee and HydraDB give us the show's rules and prove where those rules came from.
hotdata.dev checks what is true right now — is Ravi's presentation actually there?
RocketRide runs the sequence end to end. And the plan comes back to the operator
with its three cues visible.

The operator reads them. The operator approves. Only then does it run.

Introduction. Presentation. Holding. Three new receipts, for a new speaker, from
a procedure nobody had to rebuild.

**Alex.** Now the interesting one. Alex's presentation is missing.

We create his run — and it stops. Before a single cue. Zero receipts. The stage
stays on holding, and it tells the operator exactly why.

And then the important detail. We put Alex's presentation back. The blocked run
does **not** wake back up. You approve a fresh plan, or nothing moves.

## 4. Why the blocking is the whole point (~45 seconds)

Anyone can record a sequence and play it back. That is a macro.

A macro fires into an empty room. It does not know the slides are gone. It does
not know the show was edited five minutes ago. It just runs, and the audience
watches it fail.

Greenroom checks five things before it commits any cue: the approval hash, the
show revision, asset readiness, cue order, and who owns the stage. If any one of
those is wrong, that cue is rejected and the stage holds.

Reuse saves the work. The checks and the approval still control the show.

## 5. What is actually real (~30 seconds)

I want to be precise about what we verified.

On September 11th we ran the full live acceptance. Maya's learning run. Ravi's
replay with three new receipts. And a live interruption where the presentation
disappeared after the introduction — one receipt kept, the next cue blocked, stage
held.

The API owns the truth about what is on stage. The browser never guesses.

Our security scan shows zero findings in our own source. Six dependency advisories
are still open, and they are written down in the repo.

We are not claiming Greenroom makes shows faster or cheaper. We have not measured
that. What we are claiming is this: rehearse once, reuse safely, and have a
receipt for every single thing that went on that stage.

Thank you.

---

# Q&A cheat sheet

**Isn't this just a macro / a saved script?**
A macro replays blindly. Every Greenroom cue re-checks five things before it
commits: approval hash, show revision, asset readiness, cue order, stage ownership.

**What happens if slides vanish mid-show?**
Stage goes to holding, the next cue is rejected with a reason, already-committed
receipts are kept. Restoring the asset does *not* resume the invalidated plan —
the operator approves a new one.

**Careful — there are two Alex cases. Don't mix them up.**
- Live acceptance run: presentation removed *after* introduction → 1 receipt kept, presentation cue blocked.
- Manual demo: presentation off *before* the run is created → blocked at planning, 0 receipts.

**Does it make shows faster or cheaper?**
No claim. We never measured latency or cost. The claim is safe reuse plus a full
audit trail.

**Is the demo real or faked?**
Two modes. Practice uses clearly labelled fixture plans for UI rehearsal — no
sponsor execution claimed. Live makes real sponsor calls. Fixtures are never a
silent fallback when a real request fails.

**What's the difference between "completed" and "verified"?**
Completed = the physical cues finished. Verified completion = that, *plus*
verified Rote execution, plus the outcome written back to Hydra, plus the final
RocketRide trace.

**What does each sponsor tool do? (one line each)**
- **Rote** — records the cue procedure and replays it
- **Cognee + HydraDB** — hold the show's rules and prove where they came from
- **hotdata.dev** — answers "is this true right now?"
- **RocketRide** — orchestrates the prepare → execute → verify sequence
- **Snyk** — security scanning of source and dependencies

**How is it secured?**
The operator token never enters the browser bundle — a local proxy adds it to
writes. Read-only views need no token. The public bridge exposes only health plus
six named tools; operator controls and raw evidence stay on loopback.

**Security findings?**
Latest source scan: zero findings. Six dependency advisories remain open and are
documented in SECURITY.md. We did not suppress anything to get a pass.

**Can I run this on my machine?**
Practice mode, yes, no accounts needed. Live needs your own credentials and your
own learned Rote play — plays and secrets are not in the checkout.

**Why "holding" and not just "stop"?**
Holding is a real scene the audience can see. The stage always shows something
deliberate, never a blank screen or a half-finished cue.

**What's out of scope?**
Streaming platforms, OBS, microphones, scheduling and multi-tenancy. One segment,
three scenes, one stage.
