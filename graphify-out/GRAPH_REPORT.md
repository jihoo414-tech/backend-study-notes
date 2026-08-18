# Graph Report - wiki  (2026-08-19)

## Corpus Check
- 1 files · ~15,119 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 112 nodes · 129 edges · 14 communities (11 shown, 3 thin omitted)
- Extraction: 95% EXTRACTED · 5% INFERRED · 0% AMBIGUOUS · INFERRED: 7 edges (avg confidence: 0.84)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- Community 0
- Community 1
- Community 2
- Community 3
- Community 4
- Community 5
- Community 6
- Community 7
- Community 8
- Community 9
- Community 10
- Community 11
- Community 12
- Community 13

## God Nodes (most connected - your core abstractions)
1. `고정 길이 슬라이딩 윈도우` - 12 edges
2. `정렬된 두 배열 병합과 투 포인터` - 12 edges
3. `JPA 일대다 다대일 연관관계` - 7 edges
4. `AttendanceManager` - 7 edges
5. `전화번호 목록 접두어 탐색` - 7 edges
6. `StringUtils` - 6 edges
7. `투 포인터` - 5 edges
8. `접두어 생성 후 해시 조회` - 5 edges
9. `List와 Map 선택 기준` - 5 edges
10. `Map` - 5 edges

## Surprising Connections (you probably didn't know these)
- `replaceAll isBlank isEmpty` --semantically_similar_to--> `DTO 제약 조건 검증`  [INFERRED] [semantically similar]
  java/Java 문자열 공백 처리.md → spring/Spring Bean Validation.md
- `전화번호 목록 접두어 탐색 인덱스 항목` --references--> `전화번호 목록 접두어 탐색`  [EXTRACTED]
  index.md → algorithm/전화번호 목록 접두어 탐색.md
- `데이터베이스 정규화` --conceptually_related_to--> `JPA 일대다 다대일 연관관계`  [EXTRACTED]
  database/데이터베이스 정규화.md → spring/JPA 일대다 다대일 연관관계.md
- `관계형 데이터 모델링` --conceptually_related_to--> `JPA 일대다 다대일 연관관계`  [EXTRACTED]
  database/관계형 데이터 모델링.md → spring/JPA 일대다 다대일 연관관계.md
- `Optional` --references--> `Spring Data JPA 쿼리 메서드`  [EXTRACTED]
  java/Optional.md → spring/Spring Data JPA 쿼리 메서드.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **관계형 설계에서 JPA 영속화까지** — wiki_database_relational_data_modeling, wiki_database_database_normalization, wiki_spring_jpa_relationships [EXTRACTED 1.00]
- **Attendance File Load State Replacement** — wiki_projects_mission_03_attendance_manager_load, wiki_projects_mission_03_attendance_manager_atomic_load_swap, wiki_projects_mission_03_attendance_manager_composite_key_map [EXTRACTED 1.00]
- **전화번호 접두어 탐색 전략** — wiki_algorithm_________________hashmap, wiki_algorithm_________________hashset, wiki_algorithm_________________lexicographic_sorting [EXTRACTED 1.00]
- **정렬된 두 배열의 선형 병합 패턴** — wiki_algorithm_sorted_two_array_merge_two_pointer_sorted_input, wiki_algorithm_sorted_two_array_merge_two_pointer_two_pointer, wiki_algorithm_sorted_two_array_merge_two_pointer_loop_invariant, wiki_algorithm_sorted_two_array_merge_two_pointer_sorted_array_merge [EXTRACTED 1.00]
- **고정 길이 구간 합 계산 접근법** — wiki_algorithm_fixed_length_sliding_window_maximum_fixed_window_sum, wiki_algorithm_fixed_length_sliding_window_brute_force_window_recomputation, wiki_algorithm_fixed_length_sliding_window_prefix_sum_alternative [EXTRACTED 1.00]
- **Spring Data Access Flow** — wiki_spring_data_jpa_query_methods, wiki_java_optional, wiki_spring_jpa_relationships, wiki_spring_jpa_transaction_flush [INFERRED 0.85]

## Communities (14 total, 3 thin omitted)

### Community 0 - "Community 0"
Cohesion: 0.18
Nodes (14): 함수 종속과 제3정규형, 데이터베이스 정규화, 데이터베이스 정규화 연습문제, 온라인 강의 수강 신청 정규화, 관계형 데이터 모델링, ERD Cardinality Optionality Keys, Optional, 값의 부재 처리 (+6 more)

### Community 1 - "Community 1"
Cohesion: 0.18
Nodes (13): 접두어 생성 후 해시 조회, HashMap, HashSet, 사전순 정렬 후 인접 비교, List와 Map 선택 기준, 모든 전화번호 쌍 접두어 비교, 전화번호 목록 접두어 탐색, 프로그래머스 42577 전화번호 목록 (+5 more)

### Community 2 - "Community 2"
Cohesion: 0.23
Nodes (13): 슬라이딩 윈도우 경계 조건, 구간 합 단순 재계산, O(1) 보조 공간, 고정 길이 슬라이딩 윈도우, 고정 윈도우 증분 갱신, 입력 배열 포함 O(N) 공간 복잡도, O(N) 시간 복잡도, 슬라이딩 윈도우 반복문 불변식 (+5 more)

### Community 3 - "Community 3"
Cohesion: 0.19
Nodes (13): Validate-then-swap File Loading, AttendanceKey, AttendanceManager, Composite-key Attendance Map, mission-03 Attendance Manager, isPresent, List versus Map Selection Criteria, load (+5 more)

### Community 4 - "Community 4"
Cohesion: 0.27
Nodes (12): 정렬된 두 배열 병합과 투 포인터, int[], O(N + M) 시간 복잡도, 병합 반복문의 불변식, 병합 정렬의 병합 단계, 단조성, O((N + M) log(N + M)) 시간 복잡도, 배열 결합 후 재정렬 (+4 more)

### Community 5 - "Community 5"
Cohesion: 0.18
Nodes (12): countWords, mission-01 Java String Utils, isPalindrome, Java String Whitespace Handling, normalizeSpaces, Null Input Validation with IllegalArgumentException, Case-insensitive Palindrome Detection, reverse (+4 more)

### Community 6 - "Community 6"
Cohesion: 0.29
Nodes (10): List와 Map 선택 기준, ArrayList, HashMap, LinkedHashMap, List, Map, mission-03 Attendance Manager, mission-04 Library Rental (+2 more)

### Community 7 - "Community 7"
Cohesion: 0.33
Nodes (6): ApplicationRunner, Spring Boot 시작 콜백, 생성자 주입, Spring Bean 등록과 의존성 주입, 프록시와 자기 호출, Transactional과 flush 흐름

### Community 8 - "Community 8"
Cohesion: 0.50
Nodes (5): 존재 조건과 전체 조건, 순서 있는 학생 쌍 완전 탐색, 2차원 배열의 학생 관계 비교, 에라토스테네스의 체, 배열을 이용한 소수 체질

### Community 9 - "Community 9"
Cohesion: 0.50
Nodes (4): 양방향 문자열 순회, 고정 길이 분할과 진법 변환, Run-Length Encoding, 문자열 순회와 변환 패턴

### Community 10 - "Community 10"
Cohesion: 0.67
Nodes (4): Java 문자열 공백 처리, replaceAll isBlank isEmpty, Spring Bean Validation, DTO 제약 조건 검증

## Knowledge Gaps
- **33 isolated node(s):** `int[]`, `함수 종속과 제3정규형`, `온라인 강의 수강 신청 정규화`, `ERD Cardinality Optionality Keys`, `값의 부재 처리` (+28 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `고정 길이 슬라이딩 윈도우` connect `Community 2` to `Community 4`?**
  _High betweenness centrality (0.033) - this node is a cross-community bridge._
- **Why does `정렬된 두 배열 병합과 투 포인터` connect `Community 4` to `Community 2`?**
  _High betweenness centrality (0.031) - this node is a cross-community bridge._
- **What connects `int[]`, `함수 종속과 제3정규형`, `온라인 강의 수강 신청 정규화` to the rest of the system?**
  _33 weakly-connected nodes found - possible documentation gaps or missing edges._