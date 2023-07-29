# LimitedSupplyNFT
Limited Supply NFT Minting Contract with Challenge Add-on Functionality
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract FoundryCourseNft {
    uint256 public constant MAX_SUPPLY = 10000;
    uint256 public currentSupply = 0;

    mapping (address => string) public twitterHandleToOwner;
    mapping (address => uint256) public balanceOf;

    address[] public challenges;

    event Mint(address indexed receiver, string twitterHandle, uint256 tokenId);
    event ChallengeAdded(address challengeContract);

    function mintNft(address receiver, string memory twitterHandle) public payable returns (uint256) {
        require(currentSupply < MAX_SUPPLY, "Max supply reached");
        currentSupply += 1;
        twitterHandleToOwner[receiver] = twitterHandle;
        balanceOf[receiver] += 1;
        emit Mint(receiver, twitterHandle, currentSupply);
        return currentSupply;
    }

    function addChallenge(address challengeContract) public returns (address) {
        challenges.push(challengeContract);
        emit ChallengeAdded(challengeContract);
        return challengeContract;
    }

    receive() external payable {
        // This function is called for plain Ether transfers, i.e.
        // for every call with empty calldata.
    }

    fallback() external payable {
        // This function is called for all messages sent to
        // this contract (there's no other function that it could be).
    }
}
