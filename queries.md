# Code Scanning queries

## Code Scanning의 쿼리들은 크게 3가지의 사전 정의된 쿼리 suites를 가지고 있습니다.  

#### 1. code-scanning: A careful selection of highly-precise, low noise security queries.
#### 2. security-extended: All of our security queries, including code-scanning.
#### 3. security-and-quality: All of our security queries from security-extended, as well as our Code Quality queries.


  - code-scanning 쿼리스위트는 default로 사용되는 쿼리 스위트이며, 엔터프라이즈 조직내의 수백/수천개의 저장소들에 대해, GHAS기능이 과도한 alert와 그로인한 혼란과 결과의 불신(오탐)과 내부의 저항없이, 안전하게 적용될 수 있도록 매우 신중하게 선택되었고, 확장성도 면밀하게 고려된 쿼리 스위트 입니다. 
  - 개별 팀들은 각각의 속도에 맞추어 가능한 시점에, 1차결과에 따라 더욱 확대된 스위트로 확대하여 점차 분석을 확대하면서 문제점에 대한 분석을 점진적으로 깊숙히 파고 들 수 있습니다. 이 방법은 엔터프라이즈 조직에서 가장 잘 적용되며 엔터프라이즈 조직내에 서로 다른 팀들이 각각의 상태에 맞추어 DevSecOps journey를 진행하는것을 원활하게 해 줍니다. 
  - 따라서 처음 시작시 code-scanning 스위트로 출발하여, 필요시 security-extended와 security-and-quality로 확대하는 것을 권장 드립니다. 
    - 각 SAST툴마다 특징적인 차이가 있어, 예를 들면, `empty catch block`은 Code Scanning에서는 code quality 이슈로 분류되나, 다른 툴에서는 Security issue로 분류되기도 합니다. 
  - 모든 쿼리들은 오픈소스이며, CodeQL Public저장소인 https://github.com/github/codeql에서 확인 가능 합니다. 
  - 위 저장소 변경시에 언어별, Suite별로 어떤 쿼리들이 있는지를 보여주는 상세 CSV file이 생성되어 저장됩니다. 
    - [Query list](https://github.com/github/codeql/actions/workflows/query-list.yml)
