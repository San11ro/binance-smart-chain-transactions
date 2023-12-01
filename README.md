# binance-smart-chain-transactions      
The binance-smart-chain-transactions script simplifies transactions on the Binance Smart Chain (BSC), offering a straightforward tool for users to perform token transfers and interact with smart contracts. 

Utilizing the Web3.py library for BSC, this script allows users to send BNB or other BEP-20 tokens, query balances, and interact with smart contracts. It serves as a practical foundation for developers and enthusiasts engaging with the Binance Smart Chain.

from web3 import Web3

class BinanceSmartChainTransactionScript:
    def __init__(self, rpc_url, private_key):
        self.w3 = Web3(Web3.HTTPProvider(rpc_url))
        self.account = self.w3.eth.account.privateKeyToAccount(private_key)

    def send_bnb_transaction(self, to_address, amount, gas_limit=21000, gas_price=5):
        # Send BNB (native token) transaction on Binance Smart Chain
        transaction = {
            'to': to_address,
            'value': self.w3.toWei(amount, 'ether'),
            'gas': gas_limit,
            'gasPrice': self.w3.toWei(gas_price, 'gwei'),
            'nonce': self.w3.eth.getTransactionCount(self.account.address),
        }

        signed_transaction = self.w3.eth.account.signTransaction(transaction, private_key=self.account.privateKey)
        transaction_hash = self.w3.eth.sendRawTransaction(signed_transaction.rawTransaction)
        print(f"BNB Transaction sent to {to_address} with amount {amount} BNB. Transaction hash: {transaction_hash.hex()}")

    def send_token_transaction(self, token_address, to_address, amount, gas_limit=100000, gas_price=5):
        # Send BEP-20 token transaction on Binance Smart Chain
        contract = self.w3.eth.contract(address=token_address, abi=erc20_abi)
        transaction_data = contract.functions.transfer(to_address, self.w3.toWei(amount, 'ether')).buildTransaction({
            'from': self.account.address,
            'gas': gas_limit,
            'gasPrice': self.w3.toWei(gas_price, 'gwei'),
            'nonce': self.w3.eth.getTransactionCount(self.account.address),
        })

        signed_transaction = self.w3.eth.account.signTransaction(transaction_data, private_key=self.account.privateKey)
        transaction_hash = self.w3.eth.sendRawTransaction(signed_transaction.rawTransaction)
        print(f"Token Transaction sent to {to_address} with amount {amount} tokens. Transaction hash: {transaction_hash.hex()}")

# Example Usage
bsc_rpc_url = "https://bsc-dataseed.binance.org/"
your_private_key = "your_private_key_here"
erc20_abi = [...]  # Replace with the actual ABI of the BEP-20 token contract

bsc_script = BinanceSmartChainTransactionScript(bsc_rpc_url, your_private_key)

# Send BNB transaction
bsc_script.send_bnb_transaction("recipient_address_here", 0.1)

# Send BEP-20 token transaction
bsc_script.send_token_transaction("token_contract_address_here", "recipient_address_here", 100, gas_limit=150000)

This script streamlines transactions on the Binance Smart Chain, allowing users to easily send BNB or other BEP-20 token transactions. It provides a practical tool for interacting with the Binance Smart Chain, suitable for both developers and enthusiasts.






