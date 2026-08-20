# Graph Report - wiki  (2026-08-20)

## Corpus Check
- 2 files · ~20,152 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 193 nodes · 241 edges · 15 communities (12 shown, 3 thin omitted)
- Extraction: 98% EXTRACTED · 2% INFERRED · 0% AMBIGUOUS · INFERRED: 6 edges (avg confidence: 0.83)
- Token cost: 0 input · 0 output

## Community Hubs (Navigation)
- 완전탐색과 상태 정규화
- 컬렉션 선택 연습문제
- 데이터 모델링과 Spring
- 연속 중복 제거
- Java Set·Map 구현체
- 해시와 문자열 탐색
- 슬라이딩 윈도우
- 출석 관리와 복합 키
- 카드 조합 K번째 합
- 투 포인터 배열 병합
- 문자열 유틸리티
- 문자열과 DTO 검증
- HTTP 멱등성
- SQL SELECT 표현식
- Java record 동등성

## God Nodes (most connected - your core abstractions)
1. `Java Set과 Map 구현체별 시간 복잡도` - 19 edges
2. `고정 길이 슬라이딩 윈도우` - 12 edges
3. `정렬된 두 배열 병합과 투 포인터` - 12 edges
4. `방향 정규화와 완전탐색 연습문제` - 11 edges
5. `세 장 카드 합의 K번째 큰 값` - 9 edges
6. `전화번호 목록 접두어 탐색` - 8 edges
7. `List와 Map 선택 기준` - 8 edges
8. `회전 가능한 직사각형의 방향 정규화` - 7 edges
9. `긴 변·짧은 변 방향 정규화` - 7 edges
10. `완전탐색 후보 공간 축소 과정` - 7 edges

## Surprising Connections (you probably didn't know these)
- `HashMap` --semantically_similar_to--> `HashMap`  [INFERRED] [semantically similar]
  algorithm/전화번호 목록 접두어 탐색.md → java/List와 Map 선택 기준.md
- `replaceAll isBlank isEmpty` --semantically_similar_to--> `DTO 제약 조건 검증`  [INFERRED] [semantically similar]
  java/Java 문자열 공백 처리.md → spring/Spring Bean Validation.md
- `전화번호 목록 접두어 탐색` --references--> `List와 Map 선택 기준`  [EXTRACTED]
  algorithm/전화번호 목록 접두어 탐색.md → java/List와 Map 선택 기준.md
- `Java Set과 Map 구현체별 시간 복잡도` --references--> `전화번호 목록 접두어 탐색`  [EXTRACTED]
  java/Java Set과 Map 구현체별 시간 복잡도.md → algorithm/전화번호 목록 접두어 탐색.md
- `HashSet 저장 후 정렬 대안` --conceptually_related_to--> `HashSet`  [EXTRACTED]
  algorithm/세 장 카드 합의 K번째 큰 값.md → java/Java Set과 Map 구현체별 시간 복잡도.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **연속 중복 제거 구현 비교** — wiki_algorithm_consecutive_duplicate_removal_and_previous_value_comparison_adjacent_duplicate_removal, wiki_algorithm_consecutive_duplicate_removal_and_previous_value_comparison_stack_solution, wiki_algorithm_consecutive_duplicate_removal_and_previous_value_comparison_list_solution [EXTRACTED 1.00]
- **관계형 설계에서 JPA 영속화까지** — wiki_database_relational_data_modeling, wiki_database_database_normalization, wiki_spring_jpa_relationships [EXTRACTED 1.00]
- **완전탐색 후보 공간 축소 흐름** — wiki_algorithm___________________candidate_definition, wiki_algorithm___________________input_constraint_feasibility, wiki_algorithm___________________symmetry_duplicate_independence, wiki_algorithm___________________state_normalization, wiki_algorithm___________________combination_order_enumeration [EXTRACTED 1.00]
- **Java Map 구현체 비교** — wiki_java_java_set__map_____________hashmap [EXTRACTED 1.00]
- **Java Set 구현체 비교** — wiki_java_java_set__map_____________hashset, wiki_java_java_set__map_____________linkedhashset, wiki_java_java_set__map_____________treeset [EXTRACTED 1.00]
- **방향 정규화와 완전탐색 단계별 연습** — wiki_algorithm___________________desktop_cleanup, wiki_algorithm___________________mock_exam, wiki_algorithm___________________carpet, wiki_algorithm___________________prime_number_search, wiki_algorithm___________________fatigue, wiki_algorithm___________________power_grid_split [EXTRACTED 1.00]
- **전화번호 접두어 탐색 전략** — wiki_algorithm________________hashmap, wiki_algorithm________________hashset, wiki_algorithm________________lexicographic_sorting [EXTRACTED 1.00]
- **직사각형 방향 정규화와 최댓값 집계** — wiki_algorithm_____________________long_short_side_normalization, wiki_algorithm_____________________maximum_long_side, wiki_algorithm_____________________maximum_short_side, wiki_algorithm_____________________minimum_wallet_area [EXTRACTED 1.00]
- **세 카드 합의 K번째 값 탐색 흐름** — wiki_algorithm___________k_______three_index_combination, wiki_algorithm___________k_______strict_index_order, wiki_algorithm___________k_______treeset, wiki_algorithm___________k_______kth_distinct_sum [EXTRACTED 1.00]
- **Attendance File Load State Replacement** — wiki_projects_mission_03_attendance_manager_load, wiki_projects_mission_03_attendance_manager_atomic_load_swap, wiki_projects_mission_03_attendance_manager_composite_key_map [EXTRACTED 1.00]
- **정렬된 두 배열의 선형 병합 패턴** — wiki_algorithm_sorted_two_array_merge_two_pointer_sorted_input, wiki_algorithm_sorted_two_array_merge_two_pointer_two_pointer, wiki_algorithm_sorted_two_array_merge_two_pointer_loop_invariant, wiki_algorithm_sorted_two_array_merge_two_pointer_sorted_array_merge [EXTRACTED 1.00]
- **고정 길이 구간 합 계산 접근법** — wiki_algorithm_fixed_length_sliding_window_maximum_fixed_window_sum, wiki_algorithm_fixed_length_sliding_window_brute_force_window_recomputation, wiki_algorithm_fixed_length_sliding_window_prefix_sum_alternative [EXTRACTED 1.00]
- **Spring Data Access Flow** — wiki_spring_data_jpa_query_methods, wiki_java_optional, wiki_spring_jpa_relationships, wiki_spring_jpa_transaction_flush [INFERRED 0.85]

## Communities (15 total, 3 thin omitted)

### Community 0 - "완전탐색과 상태 정규화"
Cohesion: 0.07
Nodes (37): 존재 조건과 전체 조건, 순서 있는 학생 쌍 완전 탐색, 2차원 배열의 학생 관계 비교, 지갑 축 교환 대칭, 2^N 회전 조합 완전탐색, 명함별 독립 회전, 긴 변·짧은 변 방향 정규화, 긴 변들의 최댓값 (+29 more)

### Community 1 - "컬렉션 선택 연습문제"
Cohesion: 0.11
Nodes (21): 요구사항 기반 구현체 선택, Oracle Java 25 HashMap 문서, Oracle Java 25 HashSet 문서, Oracle Java 25 TreeMap 문서, Oracle Java 25 TreeSet 문서, 프로그래머스 12981 영어 끝말잇기, 프로그래머스 131127 할인 행사, 프로그래머스 131701 연속 부분 수열 합의 개수 (+13 more)

### Community 2 - "데이터 모델링과 Spring"
Cohesion: 0.12
Nodes (20): 함수 종속과 제3정규형, 데이터베이스 정규화, 데이터베이스 정규화 연습문제, 온라인 강의 수강 신청 정규화, 관계형 데이터 모델링, ERD Cardinality Optionality Keys, Optional, 값의 부재 처리 (+12 more)

### Community 3 - "연속 중복 제거"
Cohesion: 0.14
Nodes (16): 연속 중복 제거, 필요한 연산에 따른 자료구조 선택, 연속 중복 제거와 이전 값 비교, Integer 언박싱, Java Set과 Map 구현체별 시간 복잡도, O(n) 추가 공간, List와 Map 선택 기준, List 풀이 (+8 more)

### Community 4 - "Java Set·Map 구현체"
Cohesion: 0.21
Nodes (14): Comparator와 equals 일관성, 해시 기반 평균 O(1) 연산, Hash 컬렉션의 O(N + capacity) 순회, HashMap, HashSet, LinkedHashSet, Map, Set (+6 more)

### Community 5 - "해시와 문자열 탐색"
Cohesion: 0.18
Nodes (13): 접두어 생성 후 해시 조회, HashMap, HashSet, 사전순 정렬 후 인접 비교, 모든 전화번호 쌍 접두어 비교, 전화번호 목록 접두어 탐색, 프로그래머스 42577 전화번호 목록, 접두어 탐색 시간 복잡도 (+5 more)

### Community 6 - "슬라이딩 윈도우"
Cohesion: 0.23
Nodes (13): 슬라이딩 윈도우 경계 조건, 구간 합 단순 재계산, O(1) 보조 공간, 고정 길이 슬라이딩 윈도우, 고정 윈도우 증분 갱신, 입력 배열 포함 O(N) 공간 복잡도, O(N) 시간 복잡도, 슬라이딩 윈도우 반복문 불변식 (+5 more)

### Community 7 - "출석 관리와 복합 키"
Cohesion: 0.19
Nodes (13): Validate-then-swap File Loading, AttendanceKey, AttendanceManager, Composite-key Attendance Map, mission-03 Attendance Manager, isPresent, List versus Map Selection Criteria, load (+5 more)

### Community 8 - "카드 조합 K번째 합"
Cohesion: 0.20
Nodes (12): ArrayList에 모든 합 저장 후 정렬, 카드 조합 중복과 합계 값 중복의 구분, O(C(N,3) log M + K) 시간 복잡도, Collections.reverseOrder Comparator, HashSet 저장 후 정렬 대안, 중복 제거 후 K번째 큰 합, 원본 문제 메타데이터 충돌, i < j < l 인덱스 순서 (+4 more)

### Community 9 - "투 포인터 배열 병합"
Cohesion: 0.27
Nodes (12): 정렬된 두 배열 병합과 투 포인터, int[], O(N + M) 시간 복잡도, 병합 반복문의 불변식, 병합 정렬의 병합 단계, 단조성, O((N + M) log(N + M)) 시간 복잡도, 배열 결합 후 재정렬 (+4 more)

### Community 10 - "문자열 유틸리티"
Cohesion: 0.18
Nodes (12): countWords, mission-01 Java String Utils, isPalindrome, Java String Whitespace Handling, normalizeSpaces, Null Input Validation with IllegalArgumentException, Case-insensitive Palindrome Detection, reverse (+4 more)

### Community 11 - "문자열과 DTO 검증"
Cohesion: 0.67
Nodes (4): Java 문자열 공백 처리, replaceAll isBlank isEmpty, Spring Bean Validation, DTO 제약 조건 검증

## Knowledge Gaps
- **69 isolated node(s):** `존재 조건과 전체 조건`, `지갑 축 교환 대칭`, `명함별 독립 회전`, `회전 가능한 직사각형`, `탐색 후보 정의` (+64 more)
  These have ≤1 connection - possible missing edges or undocumented components.
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Java Set과 Map 구현체별 시간 복잡도` connect `컬렉션 선택 연습문제` to `카드 조합 K번째 합`, `Java Set·Map 구현체`, `해시와 문자열 탐색`?**
  _High betweenness centrality (0.168) - this node is a cross-community bridge._
- **Why does `List 풀이` connect `연속 중복 제거` to `컬렉션 선택 연습문제`, `슬라이딩 윈도우`?**
  _High betweenness centrality (0.152) - this node is a cross-community bridge._
- **Why does `ArrayList` connect `컬렉션 선택 연습문제` to `연속 중복 제거`?**
  _High betweenness centrality (0.133) - this node is a cross-community bridge._
- **What connects `존재 조건과 전체 조건`, `지갑 축 교환 대칭`, `명함별 독립 회전` to the rest of the system?**
  _69 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `완전탐색과 상태 정규화` be split into smaller, more focused modules?**
  _Cohesion score 0.06756756756756757 - nodes in this community are weakly interconnected._
- **Should `컬렉션 선택 연습문제` be split into smaller, more focused modules?**
  _Cohesion score 0.11428571428571428 - nodes in this community are weakly interconnected._
- **Should `데이터 모델링과 Spring` be split into smaller, more focused modules?**
  _Cohesion score 0.11578947368421053 - nodes in this community are weakly interconnected._