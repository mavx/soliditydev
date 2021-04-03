# soliditydev

Notes

# Setting up your environment
To setup a typical development environment, I am looking at a few things:
1. IDE that helps me code more productively
2. Development framework for common project patterns and workflows
3. Testing suite - helps make unit testing, integration testing a breeze
4. Deployment suite - helps run deployments in a predictable manner, minimizing manual human intervention

# 1. My Chosen IDE
I chose Visual Studio Code as it's my usual go-to for non-Java/Python development, just have to install the Solidity extension by Juan Blanco. This provides quite good syntax highlighting, Solidity autocompletion, compiler / linting warnings so that's pretty helpful.

# 2. Development Framework
Initially I tried truffle as it was the standard approach for end-to-end development in the past, but I found the flow clunky and iterative testing was rather slow. Plus, I experienced issues working with OpenZeppelin libraries (this was later resolved with a clean compilation), which led me to look for an alternative. So now I'm using [HardHat](https://hardhat.org/) as the taskrunner.

To set this up, install Node and complete the HardHat setup [here](https://hardhat.org/getting-started/):
```
$ brew install node
$ npm install --save-dev hardhat
```

# 3. Testing Suite
After trying out truffle with Solidity tests, and then Javascript tests, I find testing in Javascript to be more preferable as it benefits from the bigger Javascript community, so it's much easier to find examples to setup a test scenario that's beyond Solidity's or truffle's official docs.

Hardhat supports multiple test runners, [waffle](https://getwaffle.io/) looks great but since I had picked up [Mocha](https://mochajs.org/) when working with truffle earlier, I'll settle with Mocha for now.

Running tests is simple, once you have a `test/Contract.test.js` file in your project, you can either run `npx hardhat test` or `npx hardhat test test/Contract.test.js` to execute your tests. These tests will deploy your contracts into a local Ethereum node for the duration of the test so it's pretty fast, if you need to test on testnet, simply add `--network ropsten` to your CLI command, and it will pickup the necessary configurations from `hardhat.config.js`.

# 4. Deployment Suite
To deploy in HardHat, you'll need to define your deployment scripts. Here's an example:
```
const hre = require("hardhat");

async function main() {
  // We get the contract to deploy
  const Contract = await hre.ethers.getContractFactory("Contract");
  const contract = await Contract.deploy("Hello, Hardhat!");

  await contract.deployed();

  console.log("Greeter deployed to:", contract.address);
}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```

Then, run it via:
```
$ npx hardhat run scripts/sample-script.js --network ropsten
```
