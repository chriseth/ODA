# ODA

Organisation Décentralisée Autonome

This is a place to discuss requirements for a decentralised autonomous organisation,
in the sense of a smart contract anyone can invest in, while decisions about
spending the funds will be done by voting.

## General Guidelines

1. Start with wanted properties / mathematical model, analyze properties for game-theoretic or
social drawbacks. If properties turn out to be fine, write the code and show that it
fulfills the properties.

2. Cap the amount of ether that can go inside the ODA. Multiple ODAs can be created
easily if that is desired.

3. Reduce both the complexity of the implementation by using clearly isolated modules
and the complexity of the specification.

4. Add "assertions": Try to codify the properties inside the smart contract.
Simple example: The amount of ether in the contract is always equal to the
initial amount minus the sum of the ether sent to funded proposals.
If some of these assertions fail to hold, put the contract into a
"failsafe mode", which might be (a) refund-only mode or (b) redirect control
to a trusted central party.

## Desired Properties

This is a loose collection of properties the ODA should have. It remains to be seen
if these properties can be fulfilled by code on the blockchain during the
implementation stage.

Rewards are not yet part of this proposal but might be added. It has already been
shown how to add delegated voting by external contracts, so this will not be added here.

The general idea is that there is a quorum-based voting mechanism and a delay between
the end of the vote and the execution of a proposal to allow for opposing voters
to exit the DAO before their (share of the) funds are spent.

 - everyone can fund the DAO with in total up to MAX ether during the creation period of CREATION weeks,
   funded ether creates voting tokens
 - voting tokens are transferrable and fungible
 - if you interact at least once every ACTIVITY weeks, you should be able to:
   + participate in getting any proposal closer towards the quorum
   + participate in voting 'yes' or 'no' on proposals that have passed the quorum
   + exit the DAO before voted proposals start spending ether, and retrieve your remaining share of the ether
 - voting should not be strategic, assuming there are no conflicting proposals
 - proposals can be created by anyone with a voting period of VOTING days
 - vote weight should be proportional to the used voting tokens
 - votes can be changed between "yes", "no" and "abstain" any time until the end of the voting period
 - tokens can be moved, but only if all votes of these tokens are "abstain" (might be changed automatically)
 - proposal will be decided on only if at least QUORUM % of the token holders voted on it other than "abstain"
   at some point at least QUORUMDEADLINE days before the end of the voting period
 - if a proposal that has reached the quorum has a majority of "yes" votes, it can only start spending ether
   SPENDINGDELAY days after the end of its voting period
 - if your vote on a proposal at its voting deadline is "yes", your share of funds will go to the proposal (even if you exit)

## Open Questions

 - What exact mechanisms are used when you exit while you voted "yes" on a not yet executed proposal?
   Will your tokens be burnt? Can this be modeled by a simple "you can always buy back your tokens from the ODA"?
 - How to implement rewards?
   Rewards of a proposal are funds that are sent back from the proposal benefactor to the ODA. They are distributed
   to token holders that contributed to the proposal (even if they exited afterwards).
