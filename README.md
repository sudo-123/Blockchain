# Blockchain
# DAG.Sol
```
  //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;     
       contract ShortDAG{
   
        mapping(uint=> uint[])public edges;
        mapping(uint=> bool)public exists;
    
        event Node(uint id);
        event Link(uint from, uint to);
    
        function addNode(uint id) public {
            require(!exists[id],"Already Exists");
            exists[id] = true;
            emit Node(id);
        }
    
        function addEdge(uint from, uint to) public {
            require(exists[from] && exists[to],"Nodes must exists");
            require(from != to, "No self-link");
            require(!haspath(to, from), "Cycle detected");
    
            edges[from].push(to);
            emit Link(from,to);
        }
    
        function getlink(uint id)public view returns (uint[] memory) {
            return edges[id];
        }
        function haspath(uint from, uint to) internal view returns (bool) {
            if (from == to) return true;
            for (uint i = 0; i < edges[from].length; i++){
                if (haspath(edges[from][i], to)) return true;
            }
            return false;
        }
      }
```
# Hyper Ledger.Sol
```
  //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;
       contract FabricChannelCreator {
        event ChannelRequested(string channelName, string org1, string org2);
         function requestChannelCreation(
          string memory channelName,
        string memory org1,
         string memory org2
        ) public {
        emit ChannelRequested(channelName, org1, org2);
         }
      }
```
# Fact_no.Sol    
```
  //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;
       contract Factorial {
       function getFactorial(uint n) public pure returns(uint) {
          uint fact = 1;
          for(uint i = 1; i <= n; i++) {
              fact *= i;
          }
          return fact;
         }
        }
```
# Multipleaddtion.Sol
```
  //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;
      contract AddFourNumbers {
          function add(uint a, uint b, uint c, uint d) public pure returns(uint) {
              uint sum = a + b + c +d;
               return sum;
          }
        }
```
#  Natural.Sol
```
  //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;
       contract NaturalNumbers {
            function getNaturalNumbers(uint n) public pure returns(uint[] memory) {
                uint[] memory numbers = new uint[](n);
                for(uint i = 0; i < n; i++) {
                    numbers[i] = i + 1;
                }
                return numbers;
             }
         }
```
# Sum_two_no.Sol
```
   //SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;
       contract AddTwoNumbers {    
        uint public number1;
        uint public number2;
        uint public sum;
    
       function setNumbers(uint _num1, uint _num2) public {
            number1 = _num1;
            number2 = _num2;
        }
    
        function calculateSum() public {
            sum = number1 + number2;
        }
    
          function getSum() public view returns (uint) {
            return sum;
          }
       }
```
# Smart contract creation in channel
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract SimpleChannel {
    address public owner;
    uint256 public postCount;

    struct Post {
        uint256 id;
        address author;
        string content;
        uint256 timestamp;
        uint256 tipsWei;
        bool deleted;
    }

    mapping(uint256 => Post) private posts;

    event PostCreated(uint256 indexed id, address indexed author, string content, uint256 timestamp);
    event PostTipped(uint256 indexed id, address indexed tipper, uint256 amount);
    event PostDeleted(uint256 indexed id, address indexed deletedBy);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner");
        _;
    }

    modifier exists(uint256 _id) {
        require(_id > 0 && _id <= postCount, "Post does not exist");
        require(!posts[_id].deleted, "Post deleted");
        _;
    }

    constructor() {
        owner = msg.sender;
        postCount = 0;
    }

    function createPost(string calldata _content) external {
        require(bytes(_content).length > 0, "Content empty");
        postCount += 1;
        posts[postCount] = Post({
            id: postCount,
            author: msg.sender,
            content: _content,
            timestamp: block.timestamp,
            tipsWei: 0,
            deleted: false
        });
        emit PostCreated(postCount, msg.sender, _content, block.timestamp);
    }

    function tipPost(uint256 _id) external payable exists(_id) {
        require(msg.value > 0, "Tip must be > 0");
        Post storage p = posts[_id];
        p.tipsWei += msg.value;
        (bool sent, ) = p.author.call{value: msg.value}("");
        require(sent, "Transfer failed");
        emit PostTipped(_id, msg.sender, msg.value);
    }

    function deletePost(uint256 _id) external onlyOwner exists(_id) {
        posts[_id].deleted = true;
        emit PostDeleted(_id, msg.sender);
    }

    function getPost(uint256 _id) external view returns (
        uint256 id,
        address author,
        string memory content,
        uint256 timestamp,
        uint256 tipsWei,
        bool deleted
    ) {
        require(_id > 0 && _id <= postCount, "Post does not exist");
        Post storage p = posts[_id];
        return (p.id, p.author, p.content, p.timestamp, p.tipsWei, p.deleted);
    }

    function getPostsRange(uint256 from, uint256 to) external view returns (Post[] memory) {
        if (from < 1) from = 1;
        if (to > postCount) to = postCount;
        require(from <= to, "Invalid range");
        uint256 len = to - from + 1;
        Post[] memory arr = new Post[](len);
        uint256 idx = 0;
        for (uint256 i = from; i <= to; ++i) {
            arr[idx] = posts[i];
            idx++;
        }
        return arr;
    }

    receive() external payable {}
}
```

