# Lithos Overview

Lithos is a **3,3 DEX** built on the Plasma blockchain. It is designed to serve as the central liquidity layer for Plasma, providing traders, liquidity providers, and governance participants with a sustainable platform that grows alongside the ecosystem.

Unlike traditional DEXs that rely heavily on short-term liquidity incentives, Lithos is structured around the **ve(3,3) model**, which prioritizes long-term alignment between all participants. This means:

* Traders gain access to deep liquidity and low-slippage swaps.
* Liquidity providers earn both trading fees and emissions, creating consistent yield opportunities.
* veLITH holders shape the direction of the protocol, deciding how liquidity incentives are distributed.
* New projects can bootstrap liquidity and community trust through the Foundry Launchpad.

Lithos functions as Plasma’s liquidity engine, integrating trading, governance, and ecosystem expansion into a unified system.

### Core Products

#### Lithos DEX

The DEX is the foundation of the platform. Built on ve(3,3) mechanics, it ensures liquidity is both deep and sustainable.

* **Swaps with Minimal Slippage:** Traders can exchange assets across stable and volatile pools with optimized routing that reduces price impact.
* **Liquidity Provisioning:** LPs deposit assets into pools and earn yield from both trading fees and veLITH-directed emissions.
* **Transparent Analytics:** All liquidity, fees, and rewards are trackable through the dashboard, giving participants clear insight into their positions.
* **Governance Integration:** Unlike traditional DEXs, emissions are not dictated by the core team but by veLITH holders, aligning liquidity flow with community incentives.

#### Foundry Launchpad

The Foundry Launchpad acts as the entry point for new projects building on Plasma. It lowers the barrier to entry by offering both technical and liquidity support.

* **Liquidity Bootstrapping:** Projects can instantly access the Lithos DEX to establish liquid trading pairs.
* **Governance Whitelisting:** Projects can seek gauge approval, allowing them to direct emissions toward their pools, creating deeper liquidity.
* **Community Access:** Foundry projects benefit from integration into Lithos governance and the Plasma ecosystem.
* **Ecosystem Resources:** APIs, SDKs, and direct support make it easier for teams to integrate without building infrastructure from scratch.

***

### Protocol Design

<figure><img src=".gitbook/assets/2025.09.25 01-1-x-p_v0.3.png" alt=""><figcaption></figcaption></figure>

Lithos combines several mechanisms to build a liquidity system that can last through market cycles:

* **ve(3,3) Incentives:**
  * Locking LITH generates veLITH, which represents voting power.
  * veLITH holders control emissions, deciding which pools receive rewards each week.
  * The system rewards long-term commitment through extended lock periods, giving participants more influence and higher returns.
  * Emissions flow to where the community sees value, not where a central team dictates.
* **Protocol-Owned Liquidity (POL):**
  * A share of revenue is reinvested to create permanent base liquidity.
  * This reduces dependency on short-term LPs who might otherwise leave after rewards dry up.
  * POL ensures there is always liquidity for core trading pairs, supporting price stability.
* **Ignition Program:**
  * Revenue from trading fees and protocol positions is used to buy back LITH on the open market.
  * These tokens are redistributed to veLITH holders, creating a feedback loop of rewards.
  * Ignition not only incentivizes governance participation but also applies deflationary pressure on the token supply.
* **Multi-AMM Pools:**
  * **Stable Pools:** Optimized for correlated assets (e.g., USDC/USDT), offering minimal slippage.
  * **Volatile Pools:** For uncorrelated assets, allowing price discovery while still generating fees.
  * **Concentrated Liquidity Pools:** LPs can choose tighter ranges for higher efficiency, maximizing capital use.
  * **Multiple Fee Tiers:** Pools can be configured with fees appropriate to their risk level, ensuring flexibility across asset classes.

***

### Governance

Governance is the foundation of Lithos, giving token holders direct influence over how the protocol develops.

**Locking Mechanism**\
Users lock LITH to mint veLITH. Longer lock durations grant greater voting power, discouraging short-term speculation while rewarding participants who commit to the protocol.

**Gauge Voting**\
veLITH holders guide emissions through weekly gauge voting. By deciding which liquidity pools receive incentives, the community ensures that rewards remain aligned with real trading demand.

**Reward System**\
Voters are rewarded not only with emissions but also with bribes from external partners and a share of protocol revenue. Diverse reward flows give participants stronger reasons to stay engaged in governance and liquidity provision.

**Community-Led Decisions**\
Because veLITH holders control the flow of emissions, the ecosystem grows in the directions most valued by the community. This prevents centralized manipulation and distributes decision-making power across active stakeholders.

***

### Plasma Advantages

Building on the Plasma blockchain gives Lithos a set of unique benefits that strengthen its role as a 3,3 DEX.

**Gas-Free USDT Transfers**\
Plasma provides native support for USDT transfers without gas fees. Traders can move stablecoins at no extra cost, making swaps more efficient and cost-effective.

**Stablecoin-Optimized Infrastructure**\
With more than $2B in stablecoin liquidity already active, Plasma is purpose-built for stablecoin-driven markets. This foundation makes it an ideal environment for deep, reliable liquidity pools.

**High Performance**\
PlasmaBFT consensus delivers over 1,500 transactions per second. This capacity enables low-latency swaps and supports the trading volumes needed by both retail and professional users.

**DeFi Integration**\
Lithos is directly connected to more than 100 ecosystem partners, including Aave, Ethena, and Fluid. This level of integration expands utility and ensures immediate interoperability with the wider DeFi landscape.

**Security Anchored to Bitcoin**\
Plasma’s security model anchors to Bitcoin, providing resilience against network attacks. Backing from Tether, Bitfinex, and Framework Ventures further reinforces confidence in the chain’s long-term reliability.

***

### Revenue Model

The revenue framework of Lithos is built for sustainability and long-term value capture. Each stream reinforces liquidity depth, governance participation, and token demand.

**Trading Fees**

* Every swap executed on the DEX generates a fee.
* Fees are shared between liquidity providers, veLITH holders, and the protocol treasury.
* As trading activity grows, participants benefit from higher and more consistent rewards.

**veNFT Holdings**

* The foundation maintains long-term veLITH locks to remain an active stakeholder.
* veNFT positions allow the foundation to earn protocol rewards without controlling emissions directly.
* This structure aligns the foundation with community incentives while avoiding centralization.

**Ignition Program**

* A share of protocol revenue funds the Ignition Program.
* Revenue is used to purchase LITH from the open market, reducing circulating supply.
* Bought-back tokens are redistributed to veLITH holders, rewarding governance participants and driving sustained demand for LITH.
