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
