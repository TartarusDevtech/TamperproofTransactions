# Tamperproof Transactions

**About:**  
Implementation of EIP-7754 for tamperproof cryptographic transaction signing and verification.

## Overview

Tamperproof Transactions provides cryptographic signing and verification utilities compliant with [EIP-7754](https://eips.ethereum.org/EIPS/eip-7754), allowing you to sign transaction data and verify signatures using keys fetched from DNS records, JSON endpoints, or provided directly.

## Installation

```bash
yarn add @uniswap/tamperproof-transactions
# or
npm install @uniswap/tamperproof-transactions
```

## API

### SigningAlgorithm

```ts
export enum SigningAlgorithm {
  RSA = "RSA-SHA256",
  RSA_PSS = "RSA-PSS",
  ECDSA = "SHA256"
}
```

---

### `sign(data: string, privateKey: KeyObject, algorithm: SigningAlgorithm): string`

Signs a string using the provided private key and algorithm.

```ts
import { sign, SigningAlgorithm } from '@uniswap/tamperproof-transactions';
import { generateKeyPairSync } from 'crypto';

const { privateKey } = generateKeyPairSync('rsa', { modulusLength: 2048 });
const data = 'hello world';
const signature = sign(data, privateKey, SigningAlgorithm.RSA);
```

---

### `verifySync(calldata: string, signature: string, algorithm: SigningAlgorithm, publicKey: KeyObject): boolean`

Verifies a signature synchronously.

```ts
import { sign, verifySync, SigningAlgorithm } from '@uniswap/tamperproof-transactions';
import { generateKeyPairSync } from 'crypto';

const { privateKey, publicKey } = generateKeyPairSync('rsa', { modulusLength: 2048 });
const data = 'hello world';
const signature = sign(data, privateKey, SigningAlgorithm.RSA);

const isValid = verifySync(data, signature, SigningAlgorithm.RSA, publicKey);
```

---

### `verifyAsyncDns(calldata: string, signature: string, host: string, id?: number): Promise<boolean>`

Verifies a signature by fetching a public key from a DNS TXT record.

```ts
import { verifyAsyncDns } from '@uniswap/tamperproof-transactions';

const isValid = await verifyAsyncDns('data', 'signature', 'example.com');
```

---

### `verifyAsyncJson(calldata: string, signature: string, url: string, id?: number): Promise<boolean>`

Verifies a signature by fetching a public key from a JSON endpoint.

```ts
import { verifyAsyncJson } from '@uniswap/tamperproof-transactions';

const isValid = await verifyAsyncJson('data', 'signature', 'https://example.com/keys.json');
```

---

### `generate(...publicKeys: PublicKey[]): string`

Generates a JSON string containing an array of public keys.

#### Types

```ts
type PublicKey = {
  key: string;
  algorithm: SigningAlgorithm;
};
```

```ts
import { generate, SigningAlgorithm } from '@uniswap/tamperproof-transactions';

const publicKey = {
  key: 'hex-encoded-key',
  algorithm: SigningAlgorithm.RSA
};

const json = generate(publicKey);
```

---

## License

MIT
