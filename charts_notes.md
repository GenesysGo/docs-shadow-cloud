# UML for merkle tree

@startuml

class MerkleTree {
  +root: Hash
  +computeTree(data: List<Data>): void
}

class Node {
  +hash: Hash
  +left: Node
  +right: Node
  +isLeaf(): bool
}

class Data {
  +data: String
}

class Hash {
  +value: String
}

MerkleTree *--> Node
Node *--> Node
Node *--> Data
Data *--> Hash

@enduml



With our recent success in recompiling DAGGER on the Solana Saga mobile platform, our roadmap now includes the deployment of DAGGER to mobile devices, which will bring fast Layer-2 finality across mobile peers and deeper integration with Shadow Drive + Solana Mobile for our builders.

By leveraging the power of mobile resources spread across the globe, the convergence of DAGGER technology with the Shadow Platform will position us well for emerging trends such as "data-as-code," AI-edge apps, and the monetization of idle latent resources by mobile users. This will enable us to stay ahead of the curve and provide our users with cutting-edge technology for their building needs.

In summary, our successful recompilation of DAGGER on the Solana Saga mobile platform has paved the way for the deployment of DAGGER to mobile devices. This will bring fast consensus finality across mobile peers and deeper integration with the Shadow Drive, enabling us to stay ahead of emerging trends and provide our users with the latest technology for their building needs.