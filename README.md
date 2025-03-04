# 📚 스터디 라운지 (Main Page)

👋 **이곳은 면접 대비 스터디 라운지입니다.**  
우리는 다양한 기술 스택을 함께 공부하며, **실전 면접 대비**를 목표로 하고 있습니다.  
스터디원들은 자유롭게 참여하고, 자료를 공유하며 성장하는 것을 목표로 합니다. 🚀

---

## ✅ **스터디 운영 방식**
이 저장소는 **콜라보레이터(Collaborator)로 추가된 스터디원들이 직접 기여할 수 있도록 운영됩니다.**  
📌 **Fork 없이, 본 저장소에서 직접 작업**하며, 브랜치 전략을 따라 변경 내용을 관리합니다.

---

## 🚀 **브랜치 전략**
📌 **스터디 내용을 체계적으로 관리하기 위해 브랜치를 구분하여 사용합니다.**

| 브랜치                  | 역할                                             |
|----------------------|------------------------------------------------|
| `main`               | **최종 확정된 문서만 저장하는 브랜치** (직접 푸시 ❌)              |
| `study/{이름}/{학습내용토픽}` | **스터디 학습 내용 기록** (예: `study/ryu-qqq/java-gc`)  |
| `career/{이름}/{학습내용토픽}`     | **개인 경력 기술서 작성** (예: `career/ryu-qqq/java-gc`) |

---

## ✅ **작업 가이드라인**
📌 **모든 작업은 개별 브랜치에서 수행 후 `Pull Request(PR)`을 통해 `main`으로 병합됩니다.**

### 🔹 **(1) 스터디 학습 내용 추가**
1. **스터디 내용 작성용 브랜치 생성**
   ```bash
   git checkout -b study/{이름}/{학습내용토픽}
   ```
2. `/study/{이름}/{학습내용토픽}` 폴더에 Markdown 파일 추가 후 내용 작성
3. 변경 사항 커밋 후 원격 저장소로 푸시
   ```bash
   git add .
   git commit -m "스터디 내용 추가: JVM 메모리 구조"
   git push origin study/{이름}
   ```
4. **PR 생성 후 리뷰 & 병합**

---

### 🔹 **(2) 개인 경력 기술서 추가**
1. **경력 기술서 작성용 브랜치 생성**
   ```bash
   git checkout -b career/{이름}
   ```
2. `/career/{이름}/` 폴더에 개인 경력 기술서 작성
3. 변경 사항 커밋 후 푸시
   ```bash
   git add .
   git commit -m "경력 기술서 작성: 류상원"
   git push origin career/{이름}
   ```
4. **PR 생성 후 리뷰 & 병합**

---

## 📌 **스터디 관련 Wiki 문서**
📌 📖 **스터디 자료와 면접 질문은 Wiki에서 확인할 수 있습니다!**
- 추가예정..

---

## ✅ **스터디 참여 방법**
1. **콜라보레이터 추가 후, 저장소 Clone**
   ```bash
   git clone https://github.com/ryu-qqq/study-space.git
   cd study-space
   ```
2. **스터디/경력 기술서 브랜치 생성 후 작업**
3. **PR을 생성하고, 리뷰 후 병합**
4. **Wiki 및 주요 문서 업데이트**

---

## 📢 **문의 및 소통**
- 스터디 관련 질문은 **GitHub Discussions 또는 Issues**에 남겨주세요
- 직접 수정하고 싶은 내용이 있다면 **Pull Request(PR)** 부탁드립니다.
