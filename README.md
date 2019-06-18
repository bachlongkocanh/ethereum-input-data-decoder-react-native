<h3 align="center">
  <br />
  <img src="https://user-images.githubusercontent.com/168240/39537094-61c30484-4ded-11e8-8c93-410ba0510cf4.png" alt="logo" width="750" />
  <br />
  <br />
  <br />
</h3>

# ethereum-input-data-decoder

> Ethereum smart contract transaction input data decoder

[![License](http://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/miguelmota/ethereum-input-data-decoder/master/LICENSE) [![Build Status](https://travis-ci.org/miguelmota/ethereum-input-data-decoder.svg?branch=master)](https://travis-ci.org/miguelmota/ethereum-input-data-decoder) [![dependencies Status](https://david-dm.org/miguelmota/ethereum-input-data-decoder/status.svg)](https://david-dm.org/miguelmota/ethereum-input-data-decoder) [![NPM version](https://badge.fury.io/js/ethereum-input-data-decoder.svg)](http://badge.fury.io/js/ethereum-input-data-decoder)

## Demo

[https://lab.miguelmota.com/ethereum-input-data-decoder](https://lab.miguelmota.com/ethereum-input-data-decoder)

## Install

```bash
npm install ethereum-input-data-decoder
```

## Install react-native-fs
## Usage (iOS)

First you need to install react-native-fs:

```
npm install react-native-fs --save
```

**Note:** If your react-native version is < 0.40 install with this tag instead:

```
npm install react-native-fs@2.0.1-rc.2 --save
```

As @a-koka pointed out, you should then update your package.json to
`"react-native-fs": "2.0.1-rc.2"` (without the tilde)

### Adding automatically with react-native link

At the command line, in your project folder, type:

`react-native link react-native-fs`

Done! No need to worry about manually adding the library to your project.

###  Adding with CocoaPods

 Add the RNFS pod to your list of application pods in your Podfile, using the path from the Podfile to the installed module:~~

```
pod 'RNFS', :path => '../node_modules/react-native-fs'
```

Install pods as usual:
```
pod install
```

### Adding Manually in XCode

In XCode, in the project navigator, right click Libraries ➜ Add Files to [your project's name] Go to node_modules ➜ react-native-fs and add the .xcodeproj file

In XCode, in the project navigator, select your project. Add the `lib*.a` from the RNFS project to your project's Build Phases ➜ Link Binary With Libraries. Click the .xcodeproj file you added before in the project navigator and go the Build Settings tab. Make sure 'All' is toggled on (instead of 'Basic'). Look for Header Search Paths and make sure it contains both `$(SRCROOT)/../react-native/React` and `$(SRCROOT)/../../React` - mark both as recursive.

Run your project (Cmd+R)

## Usage (Android)

Android support is currently limited to only the `DocumentDirectory`. This maps to the app's `files` directory.

Make alterations to the following files:

* `android/settings.gradle`

```gradle
...
include ':react-native-fs'
project(':react-native-fs').projectDir = new File(settingsDir, '../node_modules/react-native-fs/android')
```

* `android/app/build.gradle`

```gradle
...
dependencies {
    ...
    compile project(':react-native-fs')
}
```

* register module (in MainActivity.java)

  * For react-native below 0.19.0 (use `cat ./node_modules/react-native/package.json | grep version`)

```java
import com.rnfs.RNFSPackage;  // <--- import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {

  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new RNFSPackage())      // <------- add package
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "ExampleRN", null);

    setContentView(mReactRootView);
  }

  ......

}
```

  * For react-native 0.19.0 and higher
```java
import com.rnfs.RNFSPackage; // <------- add package

public class MainActivity extends ReactActivity {
   // ...
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new MainReactPackage(), // <---- add comma
        new RNFSPackage() // <---------- add package
      );
    }
```

  * For react-native 0.29.0 and higher ( in MainApplication.java )
```java
import com.rnfs.RNFSPackage; // <------- add package

public class MainApplication extends Application implements ReactApplication {
   // ...
    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
        new MainReactPackage(), // <---- add comma
        new RNFSPackage() // <---------- add package
      );
    }
```
follow [react-native-fs](https://github.com/itinance/react-native-fs)

## Getting started

Pass [ABI](https://github.com/ethereum/wiki/wiki/Ethereum-Contract-ABI) file path to decoder constructor:

```javascript
const InputDataDecoder = require('ethereum-input-data-decoder');
const decoder = new InputDataDecoder(`${__dirname}/abi.json`);
```

Alternatively, you can pass ABI array object to constructor;

```javascript
const abi = [{ ... }]
const decoder = new InputDataDecoder(abi);
```

[example abi](./test/abi.json)

Then you can decode input data:

```javascript
const data = `0x67043cae0000000000000000000000005a9dac9315fdd1c3d13ef8af7fdfeb522db08f020000000000000000000000000000000000000000000000000000000058a20230000000000000000000000000000000000000000000000000000000000040293400000000000000000000000000000000000000000000000000000000000000a0f3df64775a2dfb6bc9e09dced96d0816ff5055bf95da13ce5b6c3f53b97071c800000000000000000000000000000000000000000000000000000000000000034254430000000000000000000000000000000000000000000000000000000000`;

const result = decoder.decodeData(data);

console.log(result);
```

```text
{
  "method": "registerOffChainDonation",
  "types": [
    "address",
    "uint256",
    "uint256",
    "string",
    "bytes32"
    ],
    "inputs": [
      <BN: 5a9dac9315fdd1c3d13ef8af7fdfeb522db08f02>,
      <BN: 58a20230>,
      <BN: 402934>,
      "BTC",
      <Buffer f3 df ... 71 c8>
    ],
    "names": [
      "addr",
      "timestamp",
      "chfCents",
      "currency",
      "memo"
    ]
}
```

Example using input response from [web3.getTransaction](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgettransaction):

```javascript
web3.eth.getTransaction(txHash, (error, txResult) => {
  const result = decoder.decodeData(txResult.input);
  console.log(result);
});
```

### Decoding Big Numbers

All numbers are returned in [big number](https://github.com/indutny/bn.js) format to preserve precision.

Here's an example of how to convert the big number to a human readable format.

```js
console.log(result.inputs[0].toString(10)) // "5"
console.log(result.inputs[0].toNumber()) // 55
```

Please keep in mind that JavaScript only supports numbers up to 64 bits. Solidity numbers can be up to 256 bits, so you run the risk of truncation when casting or having the big number library error out when trying to parse a large number to a JavaScript Number type.

```js
const n = new BN("543534254523452352345234523455")
console.log(n.toString(10)) // "543534254523452352345234523455"
console.log(n.toNumber()) // ERROR!
```

## CLI

## Install

```bash
npm install -g ethereum-input-data-decoder
```

### Usage

```bash
$ ethereum_input_data_decoder --help

  Ethereum smart contract transaction input data decoder

  Usage
    $ ethereum_input_data_decoder [flags] [input]

  Options
    --abi, -a  ABI file path
    --input, -i Input data file path

  Examples
    $ ethereum_input_data_decoder --abi token.abi --input data.txt
    $ ethereum_input_data_decoder --abi token.abi "0x23b872dd..."
```

### Example

Pass ABI file and input data as file:

```bash
$ ethereum_input_data_decoder --abi abi.json --input data.tx

method   registerOffChainDonation

address  addr       0x5a9dac9315fdd1c3d13ef8af7fdfeb522db08f02
uint256  timestamp  1487012400
uint256  chfCents   4204852
string   currency   BTC
bytes32  memo       0xf3df64775a2dfb6bc9e09dced96d0816ff5055bf95da13ce5b6c3f53b97071c8
```

Pass ABI file and input data as string:

```bash
$ ethereum_input_data_decoder --abi abi.json 0x67043cae0...000000

method   registerOffChainDonation

address  addr       0x5a9dac9315fdd1c3d13ef8af7fdfeb522db08f02
uint256  timestamp  1487012400
uint256  chfCents   4204852
string   currency   BTC
bytes32  memo       0xf3df64775a2dfb6bc9e09dced96d0816ff5055bf95da13ce5b6c3f53b97071c8
```

You can also pipe the input data:

```bash
$ cat data.txt | ethereum_input_data_decoder --abi abi.json

method   registerOffChainDonation

address  addr       0x5a9dac9315fdd1c3d13ef8af7fdfeb522db08f02
uint256  timestamp  1487012400
uint256  chfCents   4204852
string   currency   BTC
bytes32  memo       0xf3df64775a2dfb6bc9e09dced96d0816ff5055bf95da13ce5b6c3f53b97071c8
```

## Test

```bash
npm test
```

## Development

1. Clone repository:

  ```bash
  git clone git@github.com:miguelmota/ethereum-input-data-decoder.git

  cd ethereum-input-data-decoder/
  ```

2. Install dependencies:

  ```bash
  npm install
  ```

3. Make changes.

4. Run tests:

  ```bash
  npm test
  ```

5. Run linter:

  ```bash
  npm run lint:fix
  ```

## FAQ

- Q: How can I retrieve the ABI?

  - A: You can generate the ABI from the solidity source files using the [Solidity Compiler](http://solidity.readthedocs.io/en/develop/installing-solidity.html).

    ```bash
    solc --abi MyContract.sol -o build
    ```

- Q: Can this library decode contract creation input data?

  - A: Yes, it can decode contract creation input data.

- Q: Does this library support ABIEncoderV2?

  - A: Yes, but it's buggy. Please submit a [bug report](https://github.com/miguelmota/ethereum-input-data-decoder/issues/new) if you encounter issues.

## License

[MIT](LICENSE)
