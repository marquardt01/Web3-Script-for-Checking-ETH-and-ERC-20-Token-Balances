# Web3-Script-for-Checking-ETH-and-ERC-20-Token-Balances
Here's an example of a useful Web3 script that allows you to retrieve the balance of ETH and ERC-20 tokens for a given Ethereum wallet
const Web3 = require('web3');
const erc20ABI = require('erc-20-abi');

// Connect to the Ethereum network
const web3 = new Web3('https://mainnet.infura.io/v3/YOUR_INFURA_PROJECT_ID');

// Wallet address to check the balance
const walletAddress = '0xYOUR_WALLET_ADDRESS';

// ERC-20 token addresses to check the balances
const tokenAddresses = [
  '0xTOKEN_ADDRESS_1',
  '0xTOKEN_ADDRESS_2',
  '0xTOKEN_ADDRESS_3'
];

// Get ETH balance
async function getEthBalance(address) {
  try {
    const balance = await web3.eth.getBalance(address);
    return web3.utils.fromWei(balance, 'ether');
  } catch (error) {
    console.error('Failed to retrieve ETH balance:', error);
    return null;
  }
}

// Get ERC-20 token balance
async function getTokenBalance(address, tokenAddress) {
  try {
    const contract = new web3.eth.Contract(erc20ABI, tokenAddress);
    const balance = await contract.methods.balanceOf(address).call();
    return web3.utils.fromWei(balance, 'ether');
  } catch (error) {
    console.error(`Failed to retrieve balance for token ${tokenAddress}:`, error);
    return null;
  }
}

// Usage example
(async () => {
  const ethBalance = await getEthBalance(walletAddress);
  console.log(`ETH balance: ${ethBalance}`);

  for (const tokenAddress of tokenAddresses) {
    const tokenBalance = await getTokenBalance(walletAddress, tokenAddress);
    console.log(`Balance for token ${tokenAddress}: ${tokenBalance}`);
  }
})();
