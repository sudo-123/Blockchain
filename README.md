# Blockchain
# DAG.Sol
-  //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;     
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
# Hyper Ledger.Sol
-  //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;
       contract FabricChannelCreator {
         event ChannelRequested(string channelName, stringorg1, string org2);
          function requestChannelCreation(
         string memory channelName,
          string memory org1,
        string memory org2
         ) public {
          emit ChannelRequested(channelName, org1,org2);
          }
      } 
# Fact_no.Sol     
-  //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;
       contract Factorial {
       function getFactorial(uint n) public pure returns(uint) {
          uint fact = 1;
          for(uint i = 1; i <= n; i++) {
              fact *= i;
          }
          return fact;
         }
        }
# Multipleaddtion.Sol
-  //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;
      contract AddFourNumbers {
          function add(uint a, uint b, uint c, uint d) public pure returns(uint) {
              uint sum = a + b + c +d;
               return sum;
          }
        }
#  Natural.Sol
-  //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;
       contract NaturalNumbers {
            function getNaturalNumbers(uint n) public pure returns(uint[] memory) {
                uint[] memory numbers = new uint[](n);
                for(uint i = 0; i < n; i++) {
                    numbers[i] = i + 1;
                }
                return numbers;
             }
         }
# Sum_two_no.Sol
-   //SPDX-License-Identifier: MIT
-     pragma solidity ^0.8.0;
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

