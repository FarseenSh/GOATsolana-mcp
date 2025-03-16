# SolanaViz MCP Server

A Model Context Protocol (MCP) server that enables Claude to access, analyze, and visualize Solana blockchain data through natural language conversations.

## Features

- **Data Analysis**: Fetch and analyze Solana blockchain data, tokens, and wallet information
- **Visualization**: Generate text-based visualizations for data exploration
- **Price Predictions**: AI-driven price forecasts for tokens
- **Security Analysis**: Evaluate security risks for tokens and protocols
- **Multi-wallet Support**: Analyze any Solana wallet, including your own

## Prerequisites

- Node.js 18+ 
- npm or pnpm
- Claude Desktop (for using the MCP server)
- API Keys:
  - BirdEye
  - CoinGecko
  - Allora
  - Helius (Solana RPC provider)

## Installation

1. Clone the repository:

```bash
git clone https://github.com/yourusername/GOATsolana-mcp.git
cd GOATsolana-mcp
```

2. Install dependencies:

```bash
npm install
# or
pnpm install
```

3. Build the project:

```bash
npm run build
# or
pnpm build
```

## Configuration

1. Create an `.env` file in the root directory with the following environment variables (or copy from `.env.example`):

```plaintext
BIRDEYE_API_KEY=your_birdeye_api_key
COINGECKO_API_KEY=your_coingecko_api_key
ALLORA_API_KEY=your_allora_api_key
HELIUS_RPC_URL=your_helius_rpc_url
DEFAULT_WALLET_ADDRESS=optional_default_wallet_address
```

2. Configure Claude Desktop to use the MCP server by creating a `claude_desktop_config.json` file in your Claude Desktop application data directory:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

Add the following content, replacing `/path/to/GOATsolana-mcp` with the absolute path to your project:

```json
{
  "mcpServers": {
    "solana-viz-mcp": {
      "command": "node",
      "args": ["--input-type=module", "/path/to/GOATsolana-mcp/build/index.js"],
      "env": {
        "BIRDEYE_API_KEY": "your_birdeye_api_key",
        "COINGECKO_API_KEY": "your_coingecko_api_key",
        "ALLORA_API_KEY": "your_allora_api_key",
        "HELIUS_RPC_URL": "your_helius_rpc_url",
        "DEFAULT_WALLET_ADDRESS": "optional_default_wallet_address"
      }
    }
  }
}
```

> **Important**: The `--input-type=module` flag is critical for Node.js to correctly load the ES module. Without this flag, Claude Desktop will encounter a "Cannot find module" error, even if the file exists.

3. Start the server directly (for testing):

```bash
npm start
```

4. Restart Claude Desktop completely after making configuration changes.

## Important Notes

### Module Loading

This project uses ES modules (import/export) instead of CommonJS (require/module.exports). The Claude Desktop configuration includes the `--input-type=module` flag which is essential for Node.js to correctly load the modules.

### Mock Implementations

The current implementation uses mock implementations for some of the GOAT SDK plugins. This is because of interface mismatches between the plugin APIs and the expected interfaces in the code. In a future update, these mocks will be replaced with adapters to the real implementations.

## Available Tools

### Configuration Tools

- `set_wallet`: Set or change the active wallet address for analysis

### Data Fetching Tools

- `get_token_data`: Retrieve comprehensive token information and metrics
- `get_wallet_tokens`: Analyze token holdings in a wallet address
- `get_price_history`: Fetch historical price data for a token
- `get_token_pairs`: Analyze trading pairs across DEXes
- `get_trending_tokens`: Identify currently trending tokens on Solana

### Prediction Tools

- `get_price_prediction`: Generate AI-driven price forecasts for tokens

### Security Tools

- `get_token_security`: Evaluate security risks for tokens and protocols

### Visualization Tools

- `generate_sankey_diagram`: Visualize flows between entities (tokens, protocols, wallets)
- `generate_treemap`: Visualize hierarchical data with nested rectangles
- `generate_heatmap`: Visualize data intensity across categories
- `generate_network_graph`: Visualize relationships and connections between entities
- `generate_radar_chart`: Compare multiple variables for multiple entities
- `generate_bubble_chart`: Visualize 3-dimensional data (x, y, size)
- `generate_temporal_heatmap`: Visualize data patterns over time periods
- `generate_donut_chart`: Visualize part-to-whole relationships

## Usage Examples

Here are some examples of how to use SolanaViz MCP with Claude:

### Set a Wallet and Analyze Holdings

```
Please set my wallet address to FtG3emhAqBC8Rr9vdCqTJHxaNNuQVt7B7HvujJrQEj7Y and analyze what tokens I'm holding.
```

### Get Token Information with Security Analysis

```
Can you tell me more about the BONK token? Is it secure to invest in?
```

### Analyze Market Trends

```
What are the trending tokens on Solana right now? Can you show me their price performance?
```

### Visualize Portfolio Allocation

```
Analyze my wallet and visualize my portfolio allocation as a donut chart.
```

### Get Price Predictions

```
What's your price prediction for SOL token in the next 24 hours?
```

## Example Outputs

Here are examples of what you can expect to see when using the different tools through Claude:

### Data Fetching Examples

#### Token Data

When you ask about a token like BONK:

```
The BONK token (address: DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263) has the following details:

📊 Market Data:
• Current Price: $0.00002981
• Market Cap: $294,152,890
• 24h Volume: $26,843,751
• 24h Change: +7.3%
• All-time High: $0.00003241 (Jan 05, 2024)

💰 Token Economics:
• Total Supply: 99,508,123,523,595
• Circulating Supply: 56,123,876,231,175
• Holders: 842,391 addresses
• Top 10 Holders Control: 47.2% of supply

🔍 Security Analysis:
• Rugpull Risk: Low
• Contract Audit: Passed (Certik)
• Liquidity Score: 9.2/10
• Community Trust: High
• Risk Assessment: Medium
```

#### Wallet Tokens Analysis

When you ask to analyze wallet holdings:

```
Wallet FtG3emhAqBC8Rr9vdCqTJHxaNNuQVt7B7HvujJrQEj7Y contains the following tokens:

1. SOL
   • Amount: 12.45 SOL
   • Value: $1,431.75
   • % of Portfolio: 68.2%

2. BONK
   • Amount: 13,521,340.93 BONK
   • Value: $402.87
   • % of Portfolio: 19.2% 

3. JTO
   • Amount: 38.72 JTO
   • Value: $264.15
   • % of Portfolio: 12.6%

Total Portfolio Value: $2,098.77
```

#### Price History Data

When requesting price history:

```
Price history for SOL over the last 30 days:

Date        | Price (USD) | 24h Volume  | Market Cap
------------|-------------|-------------|-------------
2025-03-15  | $115.32     | $1.42B      | $49.73B
2025-03-08  | $109.78     | $1.27B      | $47.38B
2025-03-01  | $103.65     | $1.15B      | $44.73B
2025-02-22  | $114.21     | $1.33B      | $49.27B
2025-02-15  | $95.83      | $0.98B      | $41.35B

The token has shown an overall upward trend with a 20.3% increase over the past 30 days, despite some volatility in the middle of the period.
```

#### Security Analysis

When asking about token security:

```
Security Analysis for BONK token (DezXAZ8z7PnrnRJjz3wXBoRgixCa6xjnB7YaB1pPB263):

✅ Contract Analysis:
• No backdoor functions detected
• No proxy implementation risks
• No excessive owner privileges
• No unusual fee mechanisms

⚖️ Liquidity Analysis:
• Total Liquidity: $18.52M across 5 DEXes
• Largest Pool: Raydium SOL/BONK with $7.82M
• Liquidity Concentration Score: 8.4/10 (Well distributed)

🔍 Ownership Structure:
• Creator Ownership: 12% (vested over 3 years)
• DAO Treasury: 18%
• Community Allocation: 70%

⚠️ Risk Factors:
• Meme token volatility
• Significant social media influence on price
• High correlation with overall market sentiment

🔐 Overall Security Score: 8.2/10 (Low Risk)
```

### Visualization Examples

#### Donut Chart

When asking to visualize portfolio allocation:

```
Portfolio Allocation for FtG3emhAqBC8Rr9vdCqTJHxaNNuQVt7B7HvujJrQEj7Y

       ╭───────────────────────────╮
      /      SOL 68.2%              \
     /          ████████████████     \
    |           ████████████████      |
    |           ████████████████      |
    |  BONK     ████████████████  JTO |
    | 19.2% ███ ████████████████ ███  |
     \      ███ ████████████████ ███ /
      \     ███                  ███/
       ╰───███───────────────────███╯
               12.6%
               
Total Portfolio Value: $2,098.77
```

#### Heatmap

When asking to visualize token performance across timeframes:

```
Token Performance Heatmap (% Change)

           | 24h    | 7d     | 30d    | 90d    |
-----------+--------+--------+--------+--------|
SOL        | +3.2%  | +12.5% | +20.3% | +65.7% |
BONK       | +7.3%  | -3.8%  | +41.2% | +18.5% |
JTO        | -1.5%  | +5.7%  | +9.2%  | +32.1% |
PYTH       | +2.7%  | +8.2%  | +15.7% | +27.5% |
RENDER     | +0.5%  | -2.3%  | +11.3% | +42.8% |

Legend: █ >30%  █ 10-30%  █ 0-10%  █ -10-0%  █ <-10%
```

#### Network Graph

When visualizing token relationships:

```
Network Graph: Solana DeFi Token Relationships

                     ┌────────┐
                     │  SOL   │
                     └────────┘
                         │
          ┌──────────────┼──────────────┐
          │              │              │
    ┌─────▼─────┐  ┌─────▼─────┐  ┌─────▼─────┐
    │   BONK    │  │   USDC    │  │    JTO    │
    └─────┬─────┘  └─────┬─────┘  └─────┬─────┘
          │              │              │
      ┌───▼───┐      ┌───▼───┐      ┌───▼───┐
      │  wBTC │      │  ETH  │      │ PYTH  │
      └───────┘      └───────┘      └───────┘

Legend:
- Node size represents market cap
- Edge thickness represents liquidity between tokens
```

#### Sankey Diagram

When visualizing token flows:

```
Sankey Diagram: Token Flow Between Major Wallets (Last 7 Days)

   Exchange Wallets                         DeFi Protocols
┌───────────────────┐                   ┌─────────────────────┐
│                   │  12,500 SOL       │                     │
│     Binance       ├───────────────────► Marinade Finance    │
│                   │                   │                     │
└───────────────────┘                   └─────────────────────┘
                                                 │
┌───────────────────┐                            │ 9,300 SOL
│                   │  7,800 SOL         ┌───────▼─────────────┐
│     Coinbase      ├────────────────────►                     │
│                   │                    │     Orca Pools      │
└───────────────────┘                    │                     │
                                         └─────────────────────┘
┌───────────────────┐                            ▲
│                   │  15,200 SOL                │
│      Kraken       ├────────────────────────────┘
│                   │
└───────────────────┘
```

## API Key Acquisition

Here's how to obtain the required API keys:

- **BirdEye API Key**: Register at [birdeye.so](https://birdeye.so/)
- **CoinGecko API Key**: Register at [coingecko.com/en/api](https://www.coingecko.com/en/api)  
- **Allora API Key**: Register at [allora.ai](https://allora.ai/)
- **Helius RPC URL**: Register at [helius.dev](https://www.helius.dev/)

## Project Structure

The project is structured as follows:

- `src/`: Source code for the MCP server
  - `index.ts`: Main entry point for the server
  - `tools/`: Implementation of tools for various functionalities
  - `utils/`: Utility functions and helpers
  - `types/`: TypeScript type definitions
- `build/`: Compiled JavaScript code (generated after running `npm run build`)
- `.env`: Environment variables for API keys
- `claude_desktop_config.json`: Configuration for Claude Desktop

## Limitations

- Free tier API keys have rate limits that may affect functionality
- Some tools may have reduced capabilities without premium API access
- The current implementation uses mock plugins for some APIs
- Transaction execution is not supported - the server is read-only for security

## Troubleshooting

### Cannot Find Module Error

If you encounter a "Cannot find module" error in Claude Desktop, ensure that:
- The path to your index.js file in the claude_desktop_config.json is correct
- The `--input-type=module` flag is included in the args array
- You've rebuilt the project with `npm run build`
- You've restarted Claude Desktop completely

### API Key Issues

If tools fail with API errors:
- Verify that your API keys are correctly set in both `.env` and `claude_desktop_config.json`
- Check for any rate limiting on the free tiers of the APIs
- Ensure capitalization is correct (especially for BirdEye API key)

## License

MIT

## Acknowledgements

This project was built using:
- [GOAT SDK](https://github.com/goat-sdk/goat) by Crossmint
- [Model Context Protocol](https://modelcontextprotocol.io) by Anthropic