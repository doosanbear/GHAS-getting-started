# getting-started GHAS  :octocat:  
- GHAS는 GitHub의 보안 기능인 Advanced Security 기능입니다. 
- 기능은 크게 아래와 같이 구성됩니다. 
   - 의존성 보안 : 의존성 보안 확인/의존성 보안 알람/의존성 보안 자동패치(Dependabot)
   - **Code Scanning** : Code의 정적보안 분석
   - **Secret Scanning** : Credential 자동 검출 
   - Security Insight
   - Policy설정
   - [GitHub Advisory 데이터베이스](https://github.com/advisories)
- 이 중, 라이센스는 Code Scanning 과 Secret Scanning 두 가지에만 해당됩니다.

# 왜 😶❓ 
<details><summary> 🔍 </summary>
<p>

- 오픈소스 프로젝트는 이미 대세
- 상용 코드들의 90%가 오픈소스에 의존 
- 오픈소스의 Contributor 누군가가 Enterprise 상용코드에 보안 위협을 심을 수 있습니다 : **소프트웨어 공급망 공격**

  <img src="image-url" name="image-name">
  <img src="image-url" name="image-name">
  <img src="image-url" name="image-name">
</p>
</details>



# 우리의 현재는 🧐
<details><summary>🔍</summary>
<p>

* 현재 보안 상태는 🤔 
  * 현재 사용되는 도구> 좋은점.. 필요한점..  
  * 아쉬운 부분들?
* 개발자와 보안팀이 어떻게 일하나요? 🖥️
  * 어떻게 협력하죠? 
  * 문제점 발견에서 복구까지의 시간은 ? (MTTR)
  * 보안취약성을 해결하는데 얼마나 효율적으로 일하나요? ⏳
  * Dev에서 느끼는 어려운 점들은?  
  * 30일 이상 오픈되어 있는 취약성은 얼마나..?(%) 📆
* 현재의 SAST / DAST/Secret Scanning 도구들은?
  * 얼마나 오래 사용되어왔는지/어느 팀이 own
  * 좋은점과, 개선이 필요한 부분
  * 도구를 개발/관리/유지하기 위해 필요한 노력은? 
  * [No Tooling in place] What began the search for an appsec tool? 
  * [No Tooling in place] Is there something specific they're looking for in their ideal solution? 
* What languages/frameworks are in use today? (see [Supported Languages and Frameworks](https://codeql.github.com/docs/codeql-overview/supported-languages-and-frameworks/))
  * Of those you listed, which ones would you say are highest priority?
  * `Swift` is not supported today, would you say that is a dealbreaker? 
* Are there audit requirements?  If so, how often?


</p>
</details>

# 자료
- [CodeQL Document](https://codeql.github.com/docs/)
- [CodeQL Repo](https://github.com/github/codeql)
