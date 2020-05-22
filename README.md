# Karst
A distributed file system base on crust

<a href='https://web3.foundation/'><img width='320' alt='Funded by web3 foundation' src='docs/img/web3f_grants_badge.png'></a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<a href='https://builders.parity.io/'><img width='360' src='docs/img/sbp_grants_badge.png'></a>

## Compiler Environment
```shell
go version >= go1.13.4
```

# Config
Configuration file will be created by running './karst init' in $KARST_PATH/config.json (Default location is $HOME/.karst/config.json). You can change it like:

```json
{
  "base_url": "0.0.0.0:17000",
  "crust": {
    "address": "5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX",
    "backup": "{\"address\":\"5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX\",\"encoded\":\"0xc81537c9442bd1d3f4985531293d88f6d2a960969a88b1cf8413e7c9ec1d5f4955adf91d2d687d8493b70ef457532d505b9cee7a3d2b726a554242b75fb9bec7d4beab74da4bf65260e1d6f7a6b44af4505bf35aaae4cf95b1059ba0f03f1d63c5b7c3ccbacd6bd80577de71f35d0c4976b6e43fe0e1583530e773dfab3ab46c92ce3fa2168673ba52678407a3ef619b5e14155706d43bd329a5e72d36\",\"encoding\":{\"content\":[\"pkcs8\",\"sr25519\"],\"type\":\"xsalsa20-poly1305\",\"version\":\"2\"},\"meta\":{\"name\":\"Yang1\",\"tags\":[],\"whenCreated\":1580628430860}}",
    "base_url": "127.0.0.1:56666/api/v1",
    "password": "123456"
  },
  "log_level": "debug",
  "tee_base_url": "127.0.0.1:12222/api/v0"
}
```

- 'base_url' is karst url
- 'crust.address' is your chain account
- 'crust.backup' is your backup for chain
- 'crust.base_url' is crust api url for chain
- 'crust.password' is password for chain
- 'log_level' can be set as debug mode to show debug information
- 'tee_base_url' is tee base url

## Install & Run
```shell
sudo ./install.sh # for linux
```

```shell
go build # for mac and windows, then move the kasrt bin to commands folder or add it to PATH
```

```shell
karst -h
karst init #You can set $KARST_PATH to change karst installation location, default location is $Home/.karst/
karst register ws://localhost:17000 # Register your karst external address
karst daemon
karst put /home/user/file.txt --provider 5HZFQohYpN4MVyGjiq8bJhojt9yCVa8rXd4Kt9fmh5gAbQqA # put file must be absolute path
karst get f5329577a673c190b47414ddd74ce7857ea7ac6c539d0214ef245d36b2fba322 --provider 5HZFQohYpN4MVyGjiq8bJhojt9yCVa8rXd4Kt9fmh5gAbQqA --file_path /home/user/store # 'file_path' must be absolute path
```

## Websocket interface (for provider)
### Register /api/v0/cmd/register
#### Input
```json
{
	"backup": "{\"address\":\"5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX\",\"encoded\":\"0xc81537c9442bd1d3f4985531293d88f6d2a960969a88b1cf8413e7c9ec1d5f4955adf91d2d687d8493b70ef457532d505b9cee7a3d2b726a554242b75fb9bec7d4beab74da4bf65260e1d6f7a6b44af4505bf35aaae4cf95b1059ba0f03f1d63c5b7c3ccbacd6bd80577de71f35d0c4976b6e43fe0e1583530e773dfab3ab46c92ce3fa2168673ba52678407a3ef619b5e14155706d43bd329a5e72d36\",\"encoding\":{\"content\":[\"pkcs8\",\"sr25519\"],\"type\":\"xsalsa20-poly1305\",\"version\":\"2\"},\"meta\":{\"name\":\"Yang1\",\"tags\":[],\"whenCreated\":1580628430860}}",
	"password": "123",
	"karst_address": "ws://localhost:17000"
}
```

#### Return
```json
{
	"info":"Register 'ws://127.0.0.1:17000' successful in 18.624281178s ! You can check it on crust.",
	"status":200
}
```

**ps: register needs call chain's rpc, so you may wait for couple seconds waiting for chain's confirm**

## Websocket interface (for client)
### Put /api/v0/cmd/put
#### Input
```json
{
	"backup": "{\"address\":\"5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX\",\"encoded\":\"0xc81537c9442bd1d3f4985531293d88f6d2a960969a88b1cf8413e7c9ec1d5f4955adf91d2d687d8493b70ef457532d505b9cee7a3d2b726a554242b75fb9bec7d4beab74da4bf65260e1d6f7a6b44af4505bf35aaae4cf95b1059ba0f03f1d63c5b7c3ccbacd6bd80577de71f35d0c4976b6e43fe0e1583530e773dfab3ab46c92ce3fa2168673ba52678407a3ef619b5e14155706d43bd329a5e72d36\",\"encoding\":{\"content\":[\"pkcs8\",\"sr25519\"],\"type\":\"xsalsa20-poly1305\",\"version\":\"2\"},\"meta\":{\"name\":\"Yang1\",\"tags\":[],\"whenCreated\":1580628430860}}",
	"password": "123",
	"file_path": "/home/user/file.txt",
	"provider": "5HZFQohYpN4MVyGjiq8bJhojt9yCVa8rXd4Kt9fmh5gAbQqA"
}
```

**ps: 'file_path' must be absolute path**

#### Return
```json
{
	"info":"Put '/home/user/file.txt' successfully in 16.497473348s ! It root hash is '1e789508214987315bd66ed1bf7faef9e899f9cf720547ceafab6ab30a81d282'.",
	"status":200
}
```

### Get /api/v0/cmd/get
#### Input
```json
{
	"backup": "{\"address\":\"5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX\",\"encoded\":\"0xc81537c9442bd1d3f4985531293d88f6d2a960969a88b1cf8413e7c9ec1d5f4955adf91d2d687d8493b70ef457532d505b9cee7a3d2b726a554242b75fb9bec7d4beab74da4bf65260e1d6f7a6b44af4505bf35aaae4cf95b1059ba0f03f1d63c5b7c3ccbacd6bd80577de71f35d0c4976b6e43fe0e1583530e773dfab3ab46c92ce3fa2168673ba52678407a3ef619b5e14155706d43bd329a5e72d36\",\"encoding\":{\"content\":[\"pkcs8\",\"sr25519\"],\"type\":\"xsalsa20-poly1305\",\"version\":\"2\"},\"meta\":{\"name\":\"Yang1\",\"tags\":[],\"whenCreated\":1580628430860}}",
	"password": "123",
	"file_hash":     "1e789508214987315bd66ed1bf7faef9e899f9cf720547ceafab6ab30a81d282",
	"provider": "5HZFQohYpN4MVyGjiq8bJhojt9yCVa8rXd4Kt9fmh5gAbQqA",
	"file_path":     "/home/user",
}
```
**ps: 'file_path' must be absolute path**

#### Return
```json
{
	"info":"Get '1e789508214987315bd66ed1bf7faef9e899f9cf720547ceafab6ab30a81d282' successfully in 15.12343s ! Stored loaction is '/home/user' .",
	"status":200
}
```


## Websocket interface (for karst internal)

### Get /api/v0/get
#### Send message to get read permission
```json
{
	"client": "5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX",
	"store_order_hash": "5e9b98f62cfc0ca310c54958774d4b32e04d36ca84f12bd8424c1b675cf3991a",
	"file_hash": "1e789508214987315bd66ed1bf7faef9e899f9cf720547ceafab6ab30a81d282",
}
```

Success return:
```json
{
	"status": 200,
	"info": "",
	"piece_num": 14
}
```

Failed return example:
```json
{
	"status": 500,
	"info": "xxxx",
	"piece_num": 0
}
```

#### Send piece data (repeatable)
```
piece's data of the file (binary)
```

### Put /api/v0/put
#### Send order message to identity your authority
```json
{
	"cleint": "5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX",
	"store_order_hash": "5e9b98f62cfc0ca310c54958774d4b32e04d36ca84f12bd8424c1b675cf3991a",
	"merkle_tree": {
		"hash": "f5329577a673c190b47414ddd74ce7857ea7ac6c539d0214ef245d36b2fba322",
		"size": 14571017,
		"links_num": 14,
		"links": [
			{
				"hash": "e44fb124935f7efb18d7ab133c0cbedf30ae32cf11271f1bf73bd2c4d039e050",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "a76557b7d17aaa879f02a3cf7a3bdbf7e44ebddd4e418deb5c6009a3bdbff82b",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "65ff5501f376ba65d6b3afcfda79ea7b696bced7a9ff5c90dcb6c0f80517b07d",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "3c4482f9e806c3629ea4a0e6bf249f5ab79c329fcf71888731618b962123689a",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "b720abb679baefa59a49348a6582858f8a09ce882fcb65053ac0184a27b8815d",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "6e0b4a7c7197ea6c9176383ae60deb92d8144fdfe90f347e0a98eea9d7f4fce0",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "d9c0b70f037ce20d6c549326c3f804e0bdfc7dfad8838c2509e856075a69e3b7",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "085a69c8d65752c763c66820a91f21f187806390244205452638684ffd153228",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "8c2472b4026ab2461d315d46bb132bf3c889f306c36d8b3af517f0475acd9b8c",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "70d29e9031274712d50381ce2b003a7c5e4bef7406910adf9039d5c661f8cefc",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "1ed0f91ebd69aa44f5d866316436d3c8aa0b39801d24f23f846ad677b742693c",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "d239759a5b6a1e59651773d2f2db0354b36097b64736f718e50e9f29aebafb7a",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "e9a6cef50d800c7da93aae1f78a6c7abcf71aeb654d5ab7f5385195f830f69db",
				"size": 1048576,
				"links_num": 0,
				"links": []
			},
			{
				"hash": "57161f81ad5884b83cc7bc8bf2d2a6d5a4e8b318507e0001ba9b67dc45abca3a",
				"size": 939529,
				"links_num": 0,
				"links": []
			}
		]
	}
}
```
Success return:
```json
{
	"is_stored": false,
	"status": 200,
	"info": "have permission to put this file '%s'"
}
```

Failed return example:
```json
{
	"is_stored": false,
	"status": 400,
	"info": "xxxxx"
}
```

#### Send piece data (repeatable)
```
piece's data of the file (binary)
```

Success return:
```json
{
    "status": 200
}
```

Failed return example (bad request):
```json
{
    "status": 400
}
```

## Websocket interface (for TEE)
### Node data /api/v0/node/data
#### Send backup message to identity your authority
```json
{
    "backup": "{\"address\":\"5FqazaU79hjpEMiWTWZx81VjsYFst15eBuSBKdQLgQibD7CX\",\"encoded\":\"0xc81537c9442bd1d3f4985531293d88f6d2a960969a88b1cf8413e7c9ec1d5f4955adf91d2d687d8493b70ef457532d505b9cee7a3d2b726a554242b75fb9bec7d4beab74da4bf65260e1d6f7a6b44af4505bf35aaae4cf95b1059ba0f03f1d63c5b7c3ccbacd6bd80577de71f35d0c4976b6e43fe0e1583530e773dfab3ab46c92ce3fa2168673ba52678407a3ef619b5e14155706d43bd329a5e72d36\",\"encoding\":{\"content\":[\"pkcs8\",\"sr25519\"],\"type\":\"xsalsa20-poly1305\",\"version\":\"2\"},\"meta\":{\"name\":\"Yang1\",\"tags\":[],\"whenCreated\":1580628430860}}"
}
```
Success return:
```json
{
    "status": 200
}
```

Failed return example:
```json
{
    "status": 400
}
```

#### Send node get message to get node data (repeatable)
```json
{
    "file_hash": "780f2fe4461952a4fa496127a8bb79bac0957aee2739a3fb84bdc62481db6334",
    "node_hash": "f7197b8762d3a3236f8cefc3d57aaf2811a9225f7ba6490dca6c591ebed4db8c",
    "node_index": 11
}
```

Success return: data (binary)

Failed return example (bad request):
```json
{
    "status": 400
}
```

Failed return example (not found):
```json
{
    "status": 404
}
```

## Contribution

Thank you for considering to help out with the source code! We welcome contributions from anyone on the internet, and are grateful for even the smallest of fixes!

If you'd like to contribute to crust, please **fork, fix, commit and send a pull request for the maintainers to review and merge into the main codebase**.

### Rules

Please make sure your contribution adhere to our coding guideliness:

- **No --force pushes** or modifying the master branch history in any way. If you need to rebase, ensure you do it in your own repo.
- Pull requests need to be based on and opened against the `master branch`.
- A pull-request **must not be merged until CI** has finished successfully.
- Make sure your every `commit` is [signed](https://help.github.com/en/github/authenticating-to-github/about-commit-signature-verification)

### Merge process

Merging pull requests once CI is successful:

- A PR needs to be reviewed and approved by project maintainers;
- PRs that break the external API must be tagged with [`breaksapi`](https://github.com/crustio/crust-tee/labels/breakapi);
- No PR should be merged until **all reviews' comments** are addressed.

## License

[GPL v3](LICENSE)
