# Bitcoin SV MCP Server

[![smithery badge](https://smithery.ai/badge/@b-open-io/bsv-mcp)](https://smithery.ai/server/@b-open-io/bsv-mcp)

> **⚠️ NOTICE: Experimental Work in Progress**  
> This project is in an early experimental stage. Features may change, and the API is not yet stable.
> Contributions, feedback, and bug reports are welcome! Feel free to open issues or submit pull requests.

A collection of Bitcoin SV (BSV) tools for the Model Context Protocol (MCP) framework. This library provides wallet, ordinals, and utility functions for BSV blockchain interaction.

## Installation

To install dependencies:

```bash
bun install
```

## External Dependencies

For full functionality of all tools, the following additional dependencies are installed:

```bash
# For ordinal listing purchase functionality
js-1sat-ord
```

## Running the Server

Start the MCP server:

```bash
bun run index.ts
```

## Connecting to MCP Clients

This server implements the [Model Context Protocol](https://modelcontextprotocol.io/) (MCP), allowing AI assistants to utilize Bitcoin SV functionalities. You can connect this server to various MCP-compatible clients.

### Cursor

To use the BSV MCP server with [Cursor](https://cursor.sh/):

1. Install Cursor if you haven't already
2. Clone this repository and run `bun install` in the project directory
3. Open Cursor and navigate to Settings → Extensions → Model Context Protocol
4. Click "Add a new global MCP server"
5. Enter the following configuration in JSON format:

```json
{
  "Bitcoin SV": {
    "command": "env",
    "args": [
      "PRIVATE_KEY_WIF=<your_private_key_wif>",
      "bun",
      "run",
      "<path_to_project>/index.ts"
    ]
  }
}
```

6. Replace `<your_private_key_wif>` with your actual private key WIF (keep this secure!)
7. Replace `<path_to_project>` with the full path to where you cloned this repository
8. Click "Save"

The BSV tools will now be available to Cursor's AI assistant under the "Bitcoin SV" namespace.

### Claude for Desktop

To connect this server to Claude for Desktop:

1. Ensure you have [Claude for Desktop](https://claude.ai/desktop) installed and updated to the latest version
2. Clone this repository and run `bun install` in the project directory
3. Open your Claude for Desktop configuration file:
   ```bash
   # macOS/Linux
   code ~/Library/Application\ Support/Claude/claude_desktop_config.json
   
   # Windows
   code %APPDATA%\Claude\claude_desktop_config.json
   ```
4. Add the BSV MCP server to your configuration (create the file if it doesn't exist):
   ```json
   {
     "mcpServers": {
       "Bitcoin SV": {
         "command": "env",
         "args": [
           "PRIVATE_KEY_WIF=<your_private_key_wif>",
           "bun",
           "run",
           "<path_to_project>/index.ts"
         ]
       }
     }
   }
   ```
5. Replace `<your_private_key_wif>` with your actual private key WIF
6. Replace `<path_to_project>` with the full path to where you cloned this repository
7. Save the file and restart Claude for Desktop
8. The BSV tools will appear when you click the tools icon (hammer) in Claude for Desktop

### Generic MCP Client Integration

For other MCP clients that support JSON configuration:

```json
{
  "Bitcoin SV": {
    "command": "env",
    "args": [
      "PRIVATE_KEY_WIF=<your_private_key_wif>",
      "bun",
      "run",
      "<path_to_project>/index.ts"
    ]
  }
}
```

If running the server directly:

```bash
# Set environment variable first
export PRIVATE_KEY_WIF=<your_private_key_wif>

# Then run the server
bun run index.ts
```

## Available Tools

The toolkit is organized into several categories:

### Wallet Tools

Wallet tools provide core BSV wallet functionality:

| Tool Name | Description | Example Output |
|-----------|-------------|----------------|
| `wallet_getPublicKey` | Retrieves a public key for a specified protocol and key ID | `{"publicKey":"032d0c73eb9270e9e009fd1f9dd77e19cf764fbad5f799560c4e8fd414e40d6fc2"}` |
| `wallet_createSignature` | Creates a cryptographic signature for the provided data | `{"signature":[144,124,85,193,226,45,140,249,9,177,11,167,33,215,209,38,...]}` |
| `wallet_verifySignature` | Verifies a cryptographic signature against the provided data | `{"isValid":true}` |
| `wallet_encryption` | Combined tool for encrypting and decrypting data using the wallet's cryptographic keys (replaces separate encrypt/decrypt tools) | Encrypt: `{"ciphertext":[89,32,155,38,125,22,49,226,26,...]}` <br> Decrypt: `{"plaintext":[104,101,108,108,111,32,119,111,114,108,100]}` |
| `wallet_getAddress` | Returns a BSV address for the current wallet or a derived path | `{"address":"1ExampleBsvAddressXXXXXXXXXXXXXXXXX","status":"ok"}` |
| `wallet_sendToAddress` | Sends BSV to a specified address (supports BSV or USD amounts) | `{"status":"success","txid":"a1b2c3d4e5f6...","satoshis":1000000}` |
| `wallet_purchaseListing` | Purchases NFTs or BSV-20/BSV-21 tokens from marketplace listings | `{"status":"success","txid":"a1b2c3d4e5f6...","type":"nft","origin":"abcdef123456..."}` |
| `wallet_createOrdinals` | Creates and inscribes ordinals on the BSV blockchain | `{"txid":"a1b2c3d4e5f6...","inscriptionAddress":"1ExampleAddress...","contentType":"image/png"}` |

### BSV Tools

Tools for interacting with the BSV blockchain and network:

| Tool Name | Description | Example Output |
|-----------|-------------|----------------|
| `bsv_getPrice` | Gets the current BSV price from an exchange API | `Current BSV price: $38.75 USD` |
| `bsv_decodeTransaction` | Decodes a BSV transaction and returns detailed information | `{"txid":"a1b2c3d4e5f6...","version":1,"locktime":0,"size":225,"inputs":[...],"outputs":[...]}` |
| `bsv_explore` | Comprehensive blockchain explorer tool accessing WhatsOnChain API endpoints | `{"chain_info":{"chain":"main","blocks":826458,"headers":826458,"bestblockhash":"0000000000..."}}` |

### Ordinals Tools

Tools for working with ordinals (NFTs) on BSV:

| Tool Name | Description | Example Output |
|-----------|-------------|----------------|
| `ordinals_getInscription` | Retrieves detailed information about a specific inscription | `{"id":"a1b2c3d4e5f6...","origin":"a1b2c3d4e5f6...","contentType":"image/png","content":"iVBORw0KGgoAAA..."}` |
| `ordinals_searchInscriptions` | Searches for inscriptions based on various criteria | `{"results":[{"id":"a1b2c3...","contentType":"image/png","owner":"1Example..."},...]}` |
| `ordinals_marketListings` | Retrieves market listings for NFTs, BSV-20, and BSV-21 tokens with unified interface | `{"results":[{"txid":"a1b2c3...","price":9990000,"tick":"PEPE","listing":true},...]}` |
| `ordinals_marketSales` | Gets information about BSV-20 and BSV-21 token market sales | `{"results":[{"txid":"a1b2c3...","price":34710050,"tick":"$BTC","sale":true},...]}` |
| `ordinals_getTokenByIdOrTicker` | Retrieves details about a specific BSV20 token by ID | `{"tick":"PEPE","max":"21000000","lim":"1000","dec":"2"}` |

### Utility Tools

General-purpose utility functions:

| Tool Name | Description | Example Output |
|-----------|-------------|----------------|
| `utils_convertData` | Converts data between different encodings (utf8, hex, base64, binary) | `68656c6c6f20776f726c64` (UTF-8 "hello world" converted to hex) |

## Using the Tools with MCP

Once connected, you can use natural language to interact with Bitcoin SV through your AI assistant. Here are some example prompts:

### Wallet Operations
- "Get my Bitcoin SV address"
- "Send 0.01 BSV to 1ExampleBsvAddressXXXXXXXXXXXXXXXXX"
- "Send $5 USD worth of BSV to 1ExampleBsvAddressXXXXXXXXXXXXXXXXX"
- "Encrypt this message using my wallet's keys"
- "Decrypt this data that was previously encrypted for me"
- "Purchase this NFT listing: txid_vout"
- "Purchase this BSV-20 token listing: txid_vout"

### Ordinals (NFTs)
- "Show me information about the NFT with outpoint 6a89047af2cfac96da17d51ae8eb62c5f1d982be2bc4ba0d0cd2084b7ffed325_0"
- "Search for Pixel Zoide NFTs"
- "Show me the current marketplace listings for BSV NFTs"
- "Show me BSV-20 token listings for ticker PEPE"
- "Get recent BSV-20 token sales"

### Blockchain Operations
- "What is the current BSV price?"
- "Decode this BSV transaction: (transaction hex or ID)"
- "Get the latest Bitcoin SV chain information"
- "Show me block details for height 800000"
- "Explore transaction history for address 1ExampleBsvAddressXXXX"
- "Check unspent outputs (UTXOs) for my wallet address"
- "Get details for transaction with hash a1b2c3d4e5f6..."

### Data Conversion
- "Convert 'Hello World' from UTF-8 to hex format"

## How MCP Works

When you interact with an MCP-enabled AI assistant:

1. The AI analyzes your request and decides which tools to use
2. With your approval, it calls the appropriate BSV MCP tool
3. The server executes the requested operation on the Bitcoin SV blockchain
4. The results are returned to the AI assistant
5. The assistant presents the information in a natural, conversational way

## Troubleshooting

If you're having issues connecting to the server:

1. Ensure the package dependencies are properly installed: `bun install`
2. Verify your WIF private key is correctly set in the environment
3. Check that your client supports MCP and is properly configured
4. Look for error messages in the client's console output

For Claude for Desktop, check the logs at:
```bash
tail -n 20 -f ~/Library/Logs/Claude/mcp*.log
```

For Cursor, check the Cursor MCP logs in Settings → Extensions → Model Context Protocol.

## Recent Updates

- **Blockchain Explorer**: Added `bsv_explore` tool for WhatsOnChain API access with mainnet/testnet support
- **Unified Tools**: Merged `wallet_encrypt`/`wallet_decrypt` into single `wallet_encryption` tool
- **Enhanced Marketplace**: Support for NFTs, BSV-20/21 tokens in listings, sales and purchases
- **Performance**: Added price caching and optimized API endpoint structure
- **Improved Validation**: Better error handling for private keys and parameters

## Bitcoin SV Blockchain Explorer

The `bsv_explore` tool provides comprehensive access to the Bitcoin SV blockchain through the WhatsOnChain API. This powerful explorer tool allows you to query various aspects of the blockchain, including chain data, blocks, transactions, and address information.

### Available Endpoints

The tool supports the following endpoint categories and specific endpoints:

#### Chain Data
| Endpoint | Description | Required Parameters | Example Response |
|----------|-------------|---------------------|------------------|
| `chain_info` | Network statistics, difficulty, and chain work | None | `{"chain":"main","blocks":826458,"headers":826458,"bestblockhash":"000000000000..."}` |
| `chain_tips` | Current chain tips including heights and states | None | `[{"height":826458,"hash":"000000000000...","branchlen":0,"status":"active"}]` |
| `circulating_supply` | Current BSV circulating supply | None | `{"bsv":21000000}` |
| `peer_info` | Connected peer statistics | None | `[{"addr":"1.2.3.4:8333","services":"000000000000...","lastsend":1621234567}]` |

#### Block Data
| Endpoint | Description | Required Parameters | Example Response |
|----------|-------------|---------------------|------------------|
| `block_by_hash` | Complete block data via hash | `blockHash` | `{"hash":"000000000000...","confirmations":1000,"size":1000000,...}` |
| `block_by_height` | Complete block data via height | `blockHeight` | `{"hash":"000000000000...","confirmations":1000,"size":1000000,...}` |
| `tag_count_by_height` | Stats on tag count for a specific block | `blockHeight` | `{"tags":{"amp":3,"bitkey":5,"metanet":12,"planaria":7,"b":120}}` |

#### Transaction Data
| Endpoint | Description | Required Parameters | Example Response |
|----------|-------------|---------------------|------------------|
| `tx_by_hash` | Detailed transaction data | `txHash` | `{"txid":"a1b2c3d4e5f6...","version":1,"locktime":0,"size":225,...}` |
| `tx_raw` | Raw transaction hex data | `txHash` | `"01000000012345abcdef..."` |
| `tx_receipt` | Transaction receipt | `txHash` | `{"blockHash":"000000000000...","blockHeight":800000,"confirmations":26458}` |

#### Address Data
| Endpoint | Description | Required Parameters | Example Response |
|----------|-------------|---------------------|------------------|
| `address_history` | Transaction history for address | `address`, optional: `limit` | `[{"tx_hash":"a1b2c3d4e5f6...","height":800000},...]` |
| `address_utxos` | Unspent outputs for address | `address` | `[{"tx_hash":"a1b2c3d4e5f6...","tx_pos":0,"value":100000},...]` |

#### Network
| Endpoint | Description | Required Parameters | Example Response |
|----------|-------------|---------------------|------------------|
| `health` | API health check | None | `{"status":"synced"}` |

### Usage Examples

The `bsv_explore` tool can be used with natural language prompts like:

```
"Get the current Bitcoin SV blockchain information"
"Show me block #800000 details"
"Get tag count statistics for block #800000"
"Fetch transaction history for address 1ExampleBsvAddressXXXXXXXX"
"Get unspent outputs for my wallet address"
"Check transaction details for txid a1b2c3d4e5f6..."
"What is the current BSV circulating supply?"
```

Under the hood, the tool accepts parameters to specify which data to retrieve:

- `endpoint`: The specific WhatsOnChain endpoint to query (e.g., `chain_info`, `tx_by_hash`)
- `network`: The BSV network to use (`main` or `test`)
- Additional parameters as required by the specific endpoint:
  - `blockHash`: For block_by_hash endpoint
  - `blockHeight`: For block_by_height endpoint
  - `txHash`: For transaction-related endpoints
  - `address`: For address-related endpoints
  - `limit`: Optional pagination limit for address_history

### Network Options

The tool supports both mainnet and testnet:
- `main`: Bitcoin SV mainnet (default)
- `test`: Bitcoin SV testnet

## Development

This project was created using `bun init` in bun v1.2.9. [Bun](https://bun.sh) is a fast all-in-one JavaScript runtime.

### Package Configuration

The main entry point for this package is `index.ts`.

### Running Tests

```bash
bun test
```

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.