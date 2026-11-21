# BuzzPulse — Trustless L1 Buzzchain Explorer & Validator Terminal

**Live Demo:** https://mashutong.github.io/buzzpulse/

A zero-backend, zero-API-trust explorer, raw calldata unpacker, attention velocity radar, and validator seat terminal for **[Buzzchain](https://testnet.buzzchain.xyz/)** (`@buzzchain_io`) running directly against **Ethereum Sepolia L1**.

---

## Why L1-Direct (Zero Relayer API)?

Buzzchain's core architectural guarantee is that **every block can be rebuilt and verified directly from Ethereum L1 alone** without trusting the relayer API.

BuzzPulse demonstrates this end-to-end in the browser:

1. **Direct Sepolia Indexing** — Streams `BlockSealed` events and live contract state directly from `BuzzchainSequencer`, `BuzzchainValidatorVault`, `BuzzchainValidator`, and `BuzzToken`.
2. **Trustless L1 Calldata Unpacker** — Clicking any block fetches the raw `settle(BlockHeader[], bytes[])` L1 transaction calldata, unpacks every 17-byte leaf (`uint64 postId`, `uint8 kind (NEW/MATURED)`, `uint64 loudness`), and runs **4 cryptographic integrity checks** client-side:
   - `blockHash == keccak256(abi.encode(header))`
   - `dataHash == keccak256(postData)`
   - `postsRoot == OpenZeppelin StandardMerkleTree(leaves)`
   - `sum(leaf.loudness) == header.loudness`
3. **Block Maturity Breakdown** — Visualizes `NEW` vs 24h `MATURED` post leaves inside every block, highlighting mega-loudness maturity spikes with direct links to each X post.
4. **Decoded Attention Velocity Radar** — Plots hourly block rate (`blocks/h`) and loudness spikes directly from on-chain timestamps (`λ̂ = K / Δt`).
5. **Validator Seat Terminal & BGE Yield Simulator** — Inspects any Buzz Validator NFT `#ID` or EVM address (`stakerOf`, `pendingRewards`, `hasClaimed`, `$BUZZ` balance) and projects daily/30d seat yields from live 24h on-chain emissions.

---

## Ethereum Sepolia Contracts Indexed

| Contract | Address |
| :--- | :--- |
| `BuzzchainSequencer` (Proxy) | `0x44e6c7bC1146259Ff63a9b514B42C86F272631a2` |
| `BuzzToken` (`$BUZZ`) | `0xc8bdca3cFD13BcB2b1E5C4Aa3bc35C7196009e2b` |
| `BuzzchainValidator` (`BUZZV`) | `0xb5aAdd62fbC1e42bbEF02dDbBbcef053ff2acdf9` |
| `BuzzchainValidatorVault` (Proxy) | `0x5263121Df28F91EC3c2773814a7c798c0849E148` |
| `BuzzchainValidatorFaucet` | `0x0419ecE34c8124Ec121495C9E5f4956620DC6b28` |

---

## Run Locally

Zero build step required — pure static HTML + ES Modules (`ethers v6`):

```bash
python3 -m http.server 8080
# Open http://localhost:8080
```

## License

MIT
