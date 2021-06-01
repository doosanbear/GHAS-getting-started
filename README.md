# getting-started GHAS   
- GHAS는 GitHub의 보안 기능인 Advanced Security 기능입니다. 


## 왜 보안이 중요 😶❓ 
<details><summary> 🔍 </summary>
<p>

- 오픈소스 프로젝트는 이미 대세 <br>
- 상용 코드들의 90%가 오픈소스에 의존 
   ![GitHub Advanced Security_Kor (6)](https://user-images.githubusercontent.com/40287191/120053479-50842700-c065-11eb-9772-7728add02e3d.png)

- 오픈소스의 Contributor 누군가가 Enterprise 상용코드에 보안 위협을 심을 수 있습니다 : **소프트웨어 공급망 공격**
   ![Advanced Security Deck](https://user-images.githubusercontent.com/40287191/120103297-e9ac5e00-c189-11eb-96a6-e6b723b58dfe.png)

- 보안은 ** [모두의 공동책임](WhySecurity.md)** 입니다.
   
</p>
</details>

## 현재는 어떤 모습이신지요 🧐
<details><summary>🔍</summary>
<p>

* 현재 사용하시는 소스코드관리/협업 플랫폼은? 👀
   
* Devs와 Security팀이 어떻게 일하시나요? 🖥️
  * working relationship 🧑‍🤝‍🧑 : single team 처럼 함께 협력하시는지, 아니면 의사소통만 오가는 정도인지요?
  * 문제점 발견에서 복구까지의 시간은? (MTTR)
  * 보안취약성을 해결하는데 얼마나 효율적으로 일하나요? ⏳
  * Devs에서 느끼는 어려운 점들은? 
  * 30일 이상 오픈되어 있는 취약성은 얼마나..?(%) 📆
* 현재의 SAST / DAST/Secret Scanning 도구들은?🤔 
  * 얼마나 오래 사용되어왔는지/어느 팀이 ownership
  * Devs와의 워크플로우 결합은 developer integrations 또는 커밋 단계의 early integrations이 있는지요?
  * 좋은점과, 개선이 필요한 부분들이 있다면 어떤 것들이 있을까요? 👀
  * 도구를 개발/관리/유지하기 위한 현재의 노력은? 
  * 만약 현재 도구가 없다면, 무엇이, 어떤 목적 🎛️ , Initiative?

</p>
</details>


## [Why GitHub](whyGitHub.md)

## How 1. 먼저 의존성 부터 확인 합니다. 
<details><summary>🔍</summary>
<p>

* 프로젝트가 의존하고 있는 의존성은 어떤것이 있지? 🤔 : [Dependency Graph](https://github.com/doosanbear/Demo-webgoatm/network/dependencies)  
  
* 의존성에 알람이 뜨면 알람을 발생시킵니다. 🔊 : [Dependabot alert](https://github.com/doosanbear/Demo-webgoatm) 
   - Dependabot alert를 볼 수 있는 권한은 [Org owner/저장소 admin이 추가/삭제](https://docs.github.com/en/enterprise-server@3.1/github/administering-a-repository/managing-repository-settings/managing-security-and-analysis-settings-for-your-repository#granting-access-to-security-alerts)
  
* 발생된 의존성 알람에 대해 자동 패치를 수행합니다. : [Dependabot security update](https://github.com/doosanbear/Demo-webgoatm/pulls)

* 보안 데이터베이스 
   * GitHub의 전체 보안 데이터 베이스 : [GitHub Advisory Database](https://github.com/advisories)
   * GitHub은 CVE를 직접 발행할 수 있는 인증기관 (CNA: CVE Numbering Authority)
   * [NVD(National Vulnerability Database), Community Sources](https://github.blog/2019-09-18-securing-software-together/)

   
</p>
</details>

## How 2. Code를 작성할 때 들어갈 수 있는 위협요소를 분석합니다. :
<details><summary>🔍</summary>
<p>

   * GitHub + Semmle
   ![GitHub Advanced Security - issc29](https://user-images.githubusercontent.com/40287191/120106398-bf619d00-c197-11eb-8324-01691841a262.png)
   ![GitHub Advanced Security - issc29 (1)](https://user-images.githubusercontent.com/40287191/120127834-6f1c2680-c1fb-11eb-8ee1-3ae7452d2045.png)


   * [CodeQL : 정적 분석을 위한 내부 쿼리 엔진](slide/codeql.md)
   
   * [CodeQL은 Microsoft, Google, Uber등에서 분석을 위해 사용됩니다.](slide/codeql_customer.md) 
   
   * [분석예제 with Javascript](https://github.com/doosanbear/code-scanning-javascript-demo)
   
   * [Pull Request에서의 Code Scanning Alert](https://github.com/octodemo/demo-vulnerabilities-ghas/pull/21)
   
   * [더 많은 예제입니다.](https://github.com/doosanbear/Demo-webgoatm/security)
   
   * [Codeql 저장소](https://github.com/github/codeql), [Codeql-action 저장소](https://github.com/github/codeql-action)에 지속적으로 Query들이 업데이트 됩니다.
      - [GitHub Connect 설정](https://docs.github.com/en/enterprise-server@3.1/admin/github-actions/managing-access-to-actions-from-githubcom/enabling-automatic-access-to-githubcom-actions-using-github-connect)을 이용해 자동으로 업데이트된 CodeQL 쿼리들 사용가능
   
   * [순수 온프렘에서도 사용가능합니다: Codeql-action-sync-tool](https://github.com/github/codeql-action-sync-tool/)을 이용해 인터넷 연결이 없는 상황에서도 수동으로 sync가능 
   
   * 저장소의 읽기 권한이 있으면, Pull Request상에 Code Scanning 결과 볼 수 있고, 저장소에 '쓰기' 권한이 있으면 'Security' 탭에서 전체 결과 확인 가능 : [설명](https://docs.github.com/en/enterprise-server@3.1/code-security/secure-coding/automatically-scanning-your-code-for-vulnerabilities-and-errors/managing-code-scanning-alerts-for-your-repository#viewing-the-alerts-for-a-repository)
   
   * Extensive CodeQL query
   
   * [3rd 도구와 유연하게 연동하여](https://github.com/github/advanced-security-field/blob/main/technical-knowledge/code-scanning-integrations.md), [Upload SARIF](https://docs.github.com/en/code-security/secure-coding/integrating-with-code-scanning/uploading-a-sarif-file-to-github)를 통해 결과를 함께 확인할 수 있습니다.
   
   * CodeQL Visual Studio
   
   * CodeQL CLI
   
</p>
</details>

## How 3. Code 작성시 실수로 Push되는 Credential을 자동으로 검출합니다. 
<details><summary>🔍</summary>
<p>
   
   * [Secret Scanning](https://github.com/octodemo/demo-vulnerabilities-ghas/security/secret-scanning)
   * [현재 37개 패턴 coverage](https://docs.github.com/en/enterprise-server@3.1/code-security/secret-security/about-secret-scanning#about-secret-scanning-for-private-repositories)
   * GitHub.com상에 Public 저장소들은 이전부터 default로 자동 ON되어 있어왔습니다. GitHub.com상의 Private 저장소는 Organization 소속의 저장소만 지원
   * GHES는 Organization 소속의 저장소만 지원
   * User Defiend 패턴까지 지원 예정
   * Secret Scanning alert를 볼 수 있는 권한은 [Org의 Owner/저장소의 Admin이 추가/삭제 가능](https://docs.github.com/en/enterprise-server@3.1/github/administering-a-repository/managing-repository-settings/managing-security-and-analysis-settings-for-your-repository#granting-access-to-security-alerts)
   
   
</p>
</details>

## How 4. 전체적인 보안 상태를 확인할 수 있습니다. 
<details><summary>🔍</summary>
<p>
   
   * [Security Center](https://github.com/orgs/johnjohncom/security) - currently beta on GHEC
   
</p>
</details>

## How 5. Policy를 설정 합니다. 
<details><summary>🔍</summary>
<p> 
   
   * [Org에 대해 Advanced Security 강제화](https://docs.github.com/en/enterprise-server@3.1/admin/policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-advanced-security-in-your-enterprise#enforcing-a-policy-for-advanced-security-features)
   * [Policy.md 파일 설정](https://github.com/doosanbear/Demo-webgoatm/security/policy)
   

</p>
</details>

## License 🧮
<details><summary>🔍</summary>
<p>
   
* GitHub Enterprise외 추가 비용없음 
   - Dependency Graph
   - Dependabot Alert
   - Dependabot Security Update
   - Security Center
* 별도 라이센스 : GHAS license
   - Code Scanning
   - Secret Scanning
* GHAS license 라이센스 모델
   - **Commiter** 기반 : 지난 90일간 저장소에 Commit한 사람 수
   - Code Scanning은 GHAS license와 동등, Secret Scanning은 하위 기능
     - GHAS on/off : Code Scanning/Secret Scanning 모두 on/off
     - GHAS on상태에서, Secret Scanning은 Org레벨/Repo레벨에서 Code Scanning과 별도로 on/off가능하지만, GHAS를 off하면 함께 off
   - 한명의 User는 여러곳에 commit해도 '하나'로 카운트
   - 관리/비용절약 💰 
      - [Enterprise레벨에서 Org별로 enable/disable](https://docs.github.com/en/enterprise-server@3.1/admin/policies/enforcing-policies-for-your-enterprise/enforcing-policies-for-advanced-security-in-your-enterprise)
      - [Org자체에서 모든 저장소 한꺼번에 enable/disable](https://docs.github.com/en/enterprise-server@3.1/organizations/keeping-your-organization-secure/managing-security-and-analysis-settings-for-your-organization#enabling-or-disabling-a-feature-for-all-existing-repositories)
      - [저장소별로 enable/disable](https://docs.github.com/en/enterprise-server@3.1/github/administering-a-repository/managing-repository-settings/managing-security-and-analysis-settings-for-your-repository#enabling-or-disabling-security-and-analysis-features)
      - 어떤 저장소를 GHAS를 
    - Committer가 초과 시: 기존에 enable된 저장소들은 계속 추가적인 committer 허용// 신규 저장소들은 GHAS가 disable된 상태로 생성됨, GHAS가 enable되지 않았던 기존 저장소들도 GHAS의 enable버튼이 비활성화
  
   
</p>
</details>

## Trial을 원하시면 🚀
<details><summary>🔍</summary>
<p>

* GitHub.com상에 모든 Public 저장소들은 이미 사용 가능 ✨

* Private 저장소들에 대해 Trial을 하고 싶으시면 👇
   - GitHub Sales에 Trial request 📱 Trial Enable
   - 간단한 meeting : Trial 범위/대상 저장소/언어/빌드환경등,, Trial의 목적, success criteria등
   
* GitHub Enterprise Cloud에서 Trial 
   - 이미 사용중 
     - Trial하실 Enterprise Account name 이름을 보내주시면, :octocat:이 enable
     - 50 committer (만약 추가 필요하시면 Contact GitHub)
   - 사용중이 아니시면
     - 신규 Org생성, Org이름 보내주시면, :octocat:이 GitHub Enterprise Cloud Trial + GHAS Trial enable
     - 50 seat/committer (만약 추가 필요하시면 GitHub에 contact)
   
* GitHub Enterprise Server에서 Trial
   - 이미 사용중
     - Trial Enable된 라이센스를 기존 Enterprise Account의 라이센스 다운받는 곳에서 다운로드하여 적용
   - 사용중이 아니시면,
     - Trial 라이센스를 신청하신 admin분의 이메일로 전송 -> GHES설치하고 trial 라이센스 적용 
   
* 기간: 3Weeks
 * 사용방법 /매주 1~2회 call하면서 함께 진행
   
</p>
</details>

## 개발 환경 ❔
<details><summary>🔍</summary>
<p>
   
* 사용되는 languages/frameworks 🗣️ 
  * see [Supported Languages and Frameworks](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/)
  * 우선순위 🥇❔ 
   
</p>
</details>

## 자료
- [CodeQL Document](https://codeql.github.com/docs/)
- [CodeQL query help](https://codeql.github.com/codeql-query-help/)
- [push a GitHub Actions workflow to multiple repositories](https://github.com/jhutchings1/Create-ActionsPRs)

