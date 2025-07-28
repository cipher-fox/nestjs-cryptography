[![codecov](https://codecov.io/github/cipher-fox/nestjs-cryptography/branch/main/graph/badge.svg?token=I0ZFVZTREB)](https://codecov.io/github/mjorgegulab/nestjs-cryptography)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/d4c33e2a77d64f89ba7f6ba54c6d8cb5)](https://app.codacy.com/gh/cipher-fox/nestjs-cryptography/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade)
[![CodeQL](https://github.com/cipher-fox/nestjs-cryptography/actions/workflows/github-code-scanning/codeql/badge.svg?branch=main)](https://github.com/mjorgegulab/nestjs-cryptography/actions/workflows/github-code-scanning/codeql)
[![Publish Wiki](https://github.com/cipher-fox/nestjs-cryptography/actions/workflows/deploy-wiki-prod.yaml/badge.svg?branch=main)](https://nestjs-cryptography.cypher-fox.com)

# NestJS - Cryptography

<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="120" alt="Nest Logo" /></a>
</p>


## Quick Start

[Overview & Tutorial][6]

## Introduction

This library was created to address a common problem encountered when performing cryptographic operations in our projects.
It simplifies and streamlines the process, making it easier to implement secure and efficient cryptographic solutions.
Additionally, it helps avoid common mistakes,
such as the _**[reuse of initialization vectors][1]**_,
_**[reuse of encryption keys][2]**_,
or simple the use of _**[keys that are not cryptographically secure][3]**_.


Our library employs modern cryptographic standards to provide robust security and protect your data against advanced threats. We utilize a suite of trusted algorithms and practices recognized in the cryptographic community:

- **Argon2**: A cutting-edge key derivation function designed to resist GPU and ASIC attacks, making it highly effective against brute-force attempts. It offers configurable memory and time costs to balance performance and security.
- **SHA3**: The latest member of the Secure Hash Algorithm family, SHA3 provides enhanced security over its predecessors (SHA-1 and SHA-2) and is resilient against known cryptographic attacks.
- **AES-256-GCM**: Advanced Encryption Standard with a 256-bit key in Galois/Counter Mode ensures both data confidentiality and integrity. AES-256-GCM is widely used and trusted for its high level of security and performance.
- **SHAKE256**: A versatile extendable-output function (XOF) from the SHA-3 family, SHAKE256 allows for variable-length output, making it suitable for a variety of cryptographic applications like key generation and hashing.
- **HKDF-SHA3-256**: A HMAC-based Key Derivation Function using SHA3-256 as the underlying hash function. HKDF-SHA3-256 ensures secure and reliable derivation of cryptographic keys from a master secret.
- **HMAC-SHA3-256**: A mechanism for message authentication using SHA3-256. HMAC-SHA3-256 provides data integrity and authenticity by allowing verification that a message has not been altered.
- **Constant-Time Secret Comparisons**: To protect against timing attacks, our library implements constant-time algorithms for comparing secrets. This means the time taken to perform the comparison does not depend on the data being compared, preventing attackers from inferring information based on execution time.

## Installation

This package are available on the [npm][4] registry.


```bash
yarn add nestjs-cryptography
```
or
```bash
npm install nestjs-cryptography
```


## Usage on Services
To access cryptography methods from our `CryptographyService`, you could inject it using standard constructor injection

```typescript
import { Injectable } from '@nestjs/common';
import { CryptographyService } from 'nestjs-cryptography';

@Injectable()
export class SomeService {
  constructor(
    // Inject using constructor injection
    private readonly cryptographyService: CryptographyService
  ) {}

  async someMethod(): Promise<string> {
    // Access service methods
    return this.cryptographyService.genUUID();
  }
}
```


## Configuration

Once the installation is complete, the `CryptographyModule` can be configured as any other
Nest package with _`forRoot`_ or _`forRootAsync`_ methods.

You could see the complete available settings [here][5]

```typescript title="app.module.ts"
import {
  CryptographyModule,
  CryptographyOptionsInterface,
} from 'nestjs-cryptography';

@Module({
  imports: [
    CryptographyModule.forRoot<CryptographyOptionsInterface>({
      // The rest of the configuration
      encryption: {
        symmetric: {
          masterKey: '5f7f...46bf'
        }
      }
    }),
  ],
})
export class AppModule {}
```

> [!TIP]
> Like other factory providers, our factory function can be async and can inject dependencies through inject.
> For example, you mat want to get the configuration using the `ConfigurationModule`,
so to do this you should use the _`forRootAsync`_ method ⬇️⬇️⬇️.


```typescript title="app.module.ts"
import {
  CryptographyModule,
  CryptographyOptionsInterface,
} from 'nestjs-cryptography';

@Module({
  imports: [
    CryptographyModule.forRootAsync<CryptographyOptionsInterface>({
      imports: [ConfigModule],
      useFactory: async (configService: ConfigService) => ({
        // The rest of the configuration
        encryption: {
          symmetric: {
            masterKey: configService.get<string>('CRYPTOGRAPHY.MASTER_KEY')
          }
        }
      }),
      inject: [ConfigService],
    }),
  ],
})
export class AppModule {}
```


The _`forRoot()`_ and _`forRootAsync`_ method takes an options object as an argument.
These options are passed through to the underlying cryptographic operations of the instance module.

> [!NOTE]
> Please take a look at the [documentation site][6] to see the available [methods][7] and the complete [configuration][8]


[1]: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf#page=26
[2]: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf#page=27
[3]: https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-57pt1r5.pdf#page=112
[4]: https://www.npmjs.com/package/nestjs-cryptography
[5]: https://nestjs-cryptography.cypher-fox.com/docs/api-reference/settings
[6]: https://nestjs-cryptography.cypher-fox.com
[7]: https://nestjs-cryptography.cypher-fox.com/docs/category/guides
[8]: https://nestjs-cryptography.cypher-fox.com/docs/api-reference/settings
