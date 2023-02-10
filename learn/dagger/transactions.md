---
description: Every event exists
---

# this all needs redone under transactions not "events" - revie again but looking like a mass delete all

# Events Eventually Explained

### Gossip is not Drama.

I mean, it is.  But that's not the kind of gossip I'm talking about here. Gossip in the context of DAGGER is how events, operations, and data are shared among operators.&#x20;

We understand now that consensus is what makes the nodes trust each other. We understand that a graph (the DAG) of all the events is stored in the Ledger and can be verified trustlessly. But what we haven't covered is what are we reaching consensus on, specifically? And what is stored in the ledger, specifically?&#x20;

### The Event

_“What's in an event? That which we call a rug by any other event would suck just as much.” -_ William Shakespeare, Rugmeo and Juliet. Don't fact check this. This is exactly how the line was written. Trust me bro.

On a blockchain, blocks are what get cemented on the ledger. In a graph - specifically the ShadowGraph - events are what get cemented on the ledger. Every time an event gets cemented in the graph, ~~a dev gets its wings~~ the state of the ledger changes. These events are individually a representation of a change of state.

<figure><img src="../.gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

For it all to work, the contents of the "event" start with **the hashes**. Look. If you're not a giga-brain math genius, you don't need to dig too deep into these hashes. Just know that the parent hashes define a previous vector or set of events that these change from, and the self hash is just that - a hash of this event.&#x20;

In the middle of the state change is the request payload itself. You can think of this as some JSON or struct that defines the request - what size VM? What region? What's the name of the VM?

At the bottom of the event are what I would consider something like event metadata and supporting items. What are the associated transactions that led to the event? What are the timestamps? Who are the signers and where are their signatures?&#x20;

### The gossip <-> The DAG

<figure><img src="https://media.tenor.com/PFCnpKwzhL8AAAAC/dags-dogs.gif" alt=""><figcaption></figcaption></figure>

Here's a look at how Gossip drives events, which leads to a DAG. Dya like DAGs?

<figure><img src="../.gitbook/assets/image (16).png" alt=""><figcaption></figcaption></figure>

Well would you look at that. A series of interconnected, meshed events DOES in fact form a non-looping graph. And the graph can be verified and proved by the nodes, who achieved consensus on events by means of gossip. **Remember, each event represents a change in the state; hence, the progression arrows showing how state has progressed and changed with each event.**

And look - if you don't get it, that's fine, too. That just means you're a normal, red-blooded human being. I know this because anyone who understands this is actually an alien. But as a consumer, you can take solace in the fact that we are leveraging technology designed and deployed by a civilization more advanced than ours here on Earth.

