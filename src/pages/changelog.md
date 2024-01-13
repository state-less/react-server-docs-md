### v2.0.29 - 13.01.2024

* Added PostgreSQL transport to stores.
* Fixed reactivity bug

### v2.0.28
We fixed the publish mechanism to only publish when there are changes in the rendered component. This has the side-effect that you now need to set a client specific key using the `clientKey` utility. Otherwise, components will not update on every connected client. We will see if there's a way around using the utility e.g. by automatically appending client side information.

### v2.0.15
<sup>_04.05.2023_</sup>

#### Features

- Added `destroy` function to cleanup dynamic components.

#### Fixes

- Fixed a memory leak that caused dangling event listeners und additional rerenders.

#### Chore

- Updated the docs.

### v2.0.13
<sup>_27.04.2023_</sup>

#### Features

- Added reactivity across clients.

#### Fixes

- Changing a state properly rerenders the component in the context of each connected client, ensuring multi client realtime state updates.
