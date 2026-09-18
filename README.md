# KING

**Take the crown. Hold it to zero. Take the pot.**

One seat. A clock counting down. Burn $KING to take the seat off whoever is
sitting there. Still holding when the clock reaches zero, the pot is yours.

The pot is fed by trading fees on the token and sits at an address that has no
private key in existence — not the developer's, not anyone's.

**Play it:** [haydensouthall2021.github.io/king](https://haydensouthall2021.github.io/king/)
(browser demo, no wallet needed)

---

## The rules, in full

1. **Claiming burns $KING.** Tokens are destroyed against the mint with a real
   `burn` instruction. Not sent to a dead wallet, not moved to a treasury —
   removed from supply. Total supply only ever falls.

2. **Every claim resets the clock, and shortens it.** Rounds open at five
   minutes. Each claim knocks two seconds off, with a floor of thirty seconds.
   A quiet round is slow; a busy one becomes frantic.

3. **Each claim costs more than the last.** Twelve percent more, resetting at
   the start of every round.

4. **The price is a share of remaining supply, not a fixed number.** At 0.25%,
   a claim on a billion supply costs 2.5M tokens. Burn half the supply and a
   claim costs half as many. The game stays affordable no matter how much has
   been destroyed, and a large holder cannot price everyone else out.

5. **Whoever holds the seat at zero takes the whole pot.** Nothing is deducted
   from a payout.

---

## Where the money is

| | |
|---|---|
| The pot | A program-derived address. **No private key exists for it.** |
| Fees in | 85% to the pot, 10% to the project, 5% to the platform |
| Fees out | 100% of the pot to the winner |

The split is taken on the way in, is fixed in the program, and appears on every
deposit transaction. Nothing is taken from a payout.

---

## What the program cannot do

These are properties of the deployed code, not promises. Each one is checkable
in the source in a few minutes.

**Nobody can take the pot.** It lives at a PDA — an address derived from the
program itself. No private key was ever generated for it and none can be. The
only instruction that moves SOL out of it is `settle`, and `settle` can only
pay the account recorded as the current holder.

**The payout needs nobody's permission.** `settle` is permissionless. Once a
clock reaches zero, *anyone* can trigger it — and it always pays the holder,
never the person who called it. If the keeper process stops, any player can
settle the round themselves and the winner is still paid.

**Pausing cannot trap money.** The authority can stop new activity. It
deliberately has no ability to stop a payout. Check that `settle` reads no pause
flag.

**Settings freeze at launch.** Timer, price floor, step, and the fee split are
fixed when the game starts and cannot be changed afterwards. Nobody can tilt a
game that is already running, including the developer.

**There is no price oracle.** Pricing a claim against the pot's SOL value would
need an oracle, and an oracle can be manipulated for a single block and the pot
walked off with. The price is instead a share of supply plus a per-claim
ratchet — competition finds the level and nobody has to be trusted to report a
price.

---

## Verify it yourself

- **Program source:** [github.com/haydensouthall2021/arena](https://github.com/haydensouthall2021/arena)
- **21 tests**, including that two games cannot reach into each other's pots,
  that you cannot play one game with another token, that pausing cannot trap a
  payout, and that the platform authority has no route to any pot
- The pot address is published at launch. Check its balance on any explorer

The program is **multi-tenant** — $KING is the first game on it, and any token
can attach one. That is why the code lives in a separate repository.

---

## Honest limits

**Not audited.** The program is small and has been tested, but it has not had a
third-party audit. It holds real SOL. This is stated here rather than buried.

**The keeper is the one part that is not trustless.** A process watches the
clock and calls `settle` so nobody has to press anything. It cannot redirect a
single lamport — `settle` only pays the holder — but if it stops, rounds need a
player to settle them manually. The button for that is on the site.

**Fee deposits are not automatic on-chain.** Creator fees arrive in a wallet
first and are swept into the pot. That sweep is a public transaction and can be
audited by anyone. Nothing forces it to happen on a schedule.

**Nothing on a blockchain runs on a timer.** "Automatic payout" means a
transaction gets sent. The design makes sending it permissionless so it does not
matter who does.

---

## Licence

MIT. Nothing here is financial advice or an offer of anything.
