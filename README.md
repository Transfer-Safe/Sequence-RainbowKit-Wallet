# Sequence RainbowKit wallet connector

[Sequence Wallet](https://sequence.xyz/) connector for [RainbowKit](https://www.rainbowkit.com/) React library.

## Example
```TSX
import {
  ConnectButton,
  connectorsForWallets,
  wallet,
} from '@rainbow-me/rainbowkit';
import { WagmiConfig } from 'wagmi';
import { chain, configureChains, createClient } from 'wagmi';
import { alchemyProvider } from 'wagmi/providers/alchemy';
import { sequenceWallet } from 'sequence-rainbowkit-wallet';

const defaultProvider = alchemyProvider({
  apiKey: process.env.ALCHEMY_APIKEY,
});

export const { chains, provider, webSocketProvider } = configureChains(
  [chain.polygonMumbai],
  [defaultProvider],
);

const connectors = connectorsForWallets([
  {
    groupName: 'Recommended',
    wallets: [
      sequenceWallet({ chains }),
      wallet.metaMask({ chains }),
      wallet.rainbow({ chains }),
      wallet.walletConnect({ chains }),
    ],
  },
]);

const wagmiClient = createClient({
  autoConnect: true,
  connectors: () => [...connectors()],
  provider,
  webSocketProvider,
});

export const App = () => {
  return (
    <WagmiConfig client={wagmiClient}>
      <RainbowKitProvider>
        <ConnectButton />
      </RainbowKitProvider>
    </WagmiConfig>
  );
};

```
