ERC 20 Solidity COde:

```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/*
 * Install OpenZeppelin:
 * npm install @openzeppelin/contracts
 */

import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import {ERC20Burnable} from "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import {ERC20Pausable} from "@openzeppelin/contracts/token/ERC20/extensions/ERC20Pausable.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

/**
 * @title DigitalLearningHub
 * @notice Educational ERC-20 example with:
 *         - Initial fixed allocation
 *         - Owner-controlled minting
 *         - Holder-controlled burning
 *         - Owner-controlled emergency transfer pause
 *
 * @dev Uses OpenZeppelin Contracts v5.x-style Ownable constructor.
 */
contract DigitalLearningHub is ERC20, ERC20Burnable, ERC20Pausable, Ownable {
    /// @notice Maximum number of whole tokens that can ever exist.
    uint256 public constant MAX_SUPPLY = 1_000_000 * 10 ** 18;

    /**
     * @param initialOwner Address that receives ownership and the initial supply.
     *
     * The initial supply here is 100,000 tokens. The owner may mint further
     * tokens, up to MAX_SUPPLY.
     */
    constructor(address initialOwner)
        ERC20("Campus Token", "CAMP")
        Ownable(initialOwner)
    {
        _mint(initialOwner, 100_000 * 10 ** decimals());
    }

    /**
     * @notice Creates new tokens for `to`.
     * @dev Restricted to the contract owner and bounded by MAX_SUPPLY.
     *
     * @param to Recipient address.
     * @param amount Amount in smallest token units.
     */
    function mint(address to, uint256 amount) external onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Maximum supply exceeded");
        _mint(to, amount);
    }

    /**
     * @notice Pauses token transfers, minting, and burning.
     * @dev Intended as an emergency-control mechanism.
     */
    function pause() external onlyOwner {
        _pause();
    }

    /**
     * @notice Restores token transfers, minting, and burning.
     */
    function unpause() external onlyOwner {
        _unpause();
    }

    /**
     * @dev Required override so ERC20Pausable can enforce pause checks
     *      for transfers, mints, and burns in OpenZeppelin v5.
     */
    function _update(address from, address to, uint256 value)
        internal
        override(ERC20, ERC20Pausable)
    {
        super._update(from, to, value);
    }
}
```


#ERC721 - NFTs

```sol
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.24;

// OpenZeppelin ERC-721 implementation
import {ERC721, ERC721URIStorage} from "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

// Access control: only the contract owner can mint
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

contract SimpleNFT is ERC721URIStorage, Ownable {

    // Counter for generating unique token IDs
    uint256 private _nextTokenId;

    /**
     * @dev Constructor
     *
     * NFT Collection Name: CyberArt
     * NFT Symbol: CART
     *
     * msg.sender becomes the contract owner.
     */
    constructor()
        ERC721("CyberArt", "CART")
        Ownable(msg.sender)
    {
        _nextTokenId = 1;
    }

    /**
     * @dev Mint a new NFT.
     *
     * Only the contract owner can call this function.
     *
     * @param to Address that will receive the NFT
     * @param metadataURI URI pointing to the NFT metadata JSON
     *
     * Example:
     * ipfs://QmExampleHash/1.json
     */
    function mintNFT(
        address to,
        string memory metadataURI
    )
        public
        onlyOwner
        returns (uint256)
    {
        uint256 tokenId = _nextTokenId;

        // Mint the NFT
        _safeMint(to, tokenId);

        // Connect token ID to its metadata
        _setTokenURI(tokenId, metadataURI);

        // Increment ID for the next NFT
        _nextTokenId++;

        return tokenId;
    }

    /**
     * @dev Returns the next token ID that will be minted.
     */
    function nextTokenId()
        public
        view
        returns (uint256)
    {
        return _nextTokenId;
    }

    /**
     * @dev Required override because both ERC721
     * and ERC721URIStorage implement tokenURI().
     */
    function tokenURI(uint256 tokenId)
        public
        view
        override
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    /**
     * @dev Required override for ERC-165 interface support.
     */
    function supportsInterface(bytes4 interfaceId)
        public
        view
        override
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```

```
## Example Metadata
Suppose you have this image:
```cmd
cyber-warrior-1.png
```
You could create:
```cmd
1.json
```
with:
```json
{  "name": "Cyber Warrior #1",  "description": "The first CyberArt collectible representing the intersection of cybersecurity and blockchain technology.",  "image": "ipfs://YOUR_IMAGE_CID/cyber-warrior-1.png",  "attributes": [    {      "trait_type": "Class",      "value": "Warrior"    },    {      "trait_type": "Technology",      "value": "Cybersecurity"    },    {      "trait_type": "Rarity",      "value": "Rare"    },    {      "trait_type": "Edition",      "value": "Genesis"    }  ]}
```
Then upload both:
```cmd
cyber-warrior-1.png
1.json
```
to IPFS.

Your contract receives:
```
ipfs://YOUR_METADATA_CID/1.json
```
So the mint transaction becomes conceptually:
```
mintNFT(    0xYourWalletAddress,    "ipfs://QmYourMetadataCID/1.json");
```

Check CID on IPFS:
```
https://ipfs.io/ipfs/YOUR_IMAGE_CID/cyber-warrior-1.png
```

# Blockchain Security - Ethernaut CTF 

## The first **Hello Ethernaut**

- The idea for this level is to interact with the deployed contract itslef

```
contract.info()
```

```
contract.info1()
```

```
contract.info2("hello")
```

```
contract.infoNum()
```

```
contract.address
```


```
await contract.password()
```
- ethernaut0
```
await contract.authenticate("ethernaut0")
```

Submit the instance.
