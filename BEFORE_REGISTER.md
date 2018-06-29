패스트캠퍼스 EOS Smart Contract 프로그래밍 CAMP를 수강하시려면 기본적인 C++에 대한 이해가 필요합니다.

다음 문제에 대한 답을 스스로 도출해낼 수 있어야, 강의를 따라오는 데에 지장이 없습니다.

### 문제 1.

아래 코드의 출력되는 결과값 data1.name, data2.name은 무엇인가?

```cpp
	#include <iostream>

	using namespace std;

	struct Data
	{
	    char *name;
	    int number;
	};

	Data CreateData(char* _name, int _number)
	{
	    Data result;
	    result.name = new char[ strlen(_name)+1 ];
	    strcpy(result.name, _name);
	    result.number = _number;
	    return result;
	}

	int main()
	{
	    char _name[32] = "EOS";
	    
	    Data data1 = CreateData(_name, 100);
	    Data data2 = data1;
	    strcpy(data1.name, "Ethereum");
	    
	    cout<<data1.name<<endl;
	    cout<<data2.name<<endl;
	    return 0;
	}

// 정답: Ethereum, Ethereum
```

### 문제 2.

아래 코드의 출력되는 값들은 무엇인가?

```cpp
	#include <iostream>

	using namespace std;

	int main()
	{
	    char table[32] = "My Name Is Ethereum";
	    char* pChar = table;
	    char** ppChar = &pChar;

	    
	    cout<< pChar + 1   <<endl;
	    cout<< *pChar + 2  <<endl;
	    cout<< *ppChar + 3 <<endl;
	    cout<< **ppChar + 4<<endl;

	    return 0;
	}

// 정답
// y Name Is Ethereum
// 79
// Name Is Ethereum
// 81
```

### 문제 3.

아래 코드의 출력되는 값들은 무엇인가?

```cpp
	#include <iostream>

	using namespace std;


	int func1(int a)
	{
	    static int result = 0;
	    result = result+a;
	    return result;
	}
	int func2(int a)
	{
	    int result = 0;
	    result = result + func1(a);
	    return result;
	}


	int main()
	{    
	    cout<<func1(10)<<endl;
	    cout<<func2(10)<<endl;

	    cout<<func1(20)<<endl;
	    cout<<func2(20)<<endl;   
	}

// 정답
// 10
// 20
// 40
// 60
```

### 문제 4 (Optional)

다음과 같은 ERC-20 토큰 컨트랙트가 있다. 해당 토큰에서 발생할 수 있는 문제에 대하여 간단하게 서술하여라.

```solidity
	pragma solidity ^0.4.19;

	library SafeMath {
	    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
	        c = a + b;
	        require(c >= a);
	    }
	    function sub(uint256 a, uint256 b) internal pure returns (uint256 c) {
	        require(b <= a);
	        c = a - b;
	    }
	    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
	        c = a * b;
	        require(a == 0 || c / a == b);
	    }
	    function div(uint256 a, uint256 b) internal pure returns (uint256 c) {
	        require(b > 0);
	        c = a / b;
	    }
	}

	contract TestToken {
	    using SafeMath for uint256;

	    string public name;
	    string public symbol;
	    uint256 public decimals;
	    uint256 public totalSupply;

	    mapping(address => uint256) public balances;
	    mapping(address => mapping(address => uint256)) public allowed;

	    event Transfer(address indexed from, address indexed to, uint256 value);
	    event Approval(address indexed tokenOwner, address indexed spender, uint256 tokens);

	    function TinkToken() public {
	        name = "TestToken";
	        symbol = "TEST";
	        decimals = 18;        
	        totalSupply = 100000000 * 10 ** decimals;
	        balances[msg.sender] = totalSupply;
	    }

	    function balanceOf(address _owner) public constant returns (uint) {
	        return balances[_owner];
	    }

	    //
	    // To transfer token to specified address
	    //
	    function transfer(address _to, uint256 _value) public {
	        require(_to > address(0));
	        require(balances[msg.sender] >= _value);
	        require(balances[_to] + _value > balances[_to]);

	        // balances[msg.sender] -= _value
	        balances[msg.sender] = balances[msg.sender].sub(_value);
	        // balances[_to] += _value
	        balances[_to] = balances[_to].add(_value);

	        Transfer(msg.sender, _to, _value);
	    }

	    //
	    // To transfer token from source address to target address
	    //
	    function transferFrom(address _from, address _to, uint256 _value) public {
	        require(_from > address(0));
	        require(_to > address(0));
	        require(balances[_from] >= _value);
	        require(balances[_to] + _value > balances[_to]);

	        balances[_from] = balances[_from].sub(_value);
	        balances[_to] = balances[_to].add(_value);

	        Transfer(_from, _to, _value);
	    }

	    function approve(address _spender, uint256 _tokens) public returns (bool success) {
	        require(_spender > address(0));
	        allowed[msg.sender][_spender] = _tokens;
	        Approval(msg.sender, _spender, _tokens);
	        return true;
	    }
	}
```
