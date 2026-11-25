# How CLIP Works

**분류기를 학습하지 않고, 문장으로 적는다.** CLIP은 인터넷에서 모은 (이미지, 텍스트) 4억 쌍으로 "이 사진의 짝은 어느 문장인가"만 맞히도록 이미지 인코더와 텍스트 인코더를 함께 학습합니다. 짝인 이미지와 문장이 같은 임베딩 공간에서 가까워지므로, 클래스 이름을 문장으로 써서 텍스트 인코더에 넣으면 그 출력이 곧 분류기의 가중치가 됩니다. 「Learning Transferable Visual Models From Natural Language Supervision」(Radford 외, ICML 2021)을 앞에서부터 따라가며, 대조 손실을 직접 계산하고 브라우저에서 작은 CLIP을 학습시켜 제로샷 분류기를 조립해 보는 인터랙티브 교육 자료입니다.

🔗 **[데모 바로가기](https://dev-jonghoonpark.github.io/how-clip-works/)**

## 다루는 내용

1. **출발점** · 고정된 라벨 1000개의 한계, 약지도 학습과 Visual N-Grams, 자연어 지도의 빈칸
2. **데이터** · WIT 4억 쌍 — 질의어 50만 개, 질의어당 최대 2만 쌍
3. **학습 과제 고르기** · 캡션 예측 → bag-of-words 예측(3×) → 대조(다시 4×)
4. **대조 학습** · N×N 유사도 행렬과 대칭 교차 엔트로피, 그림 3 의사코드, **대조 손실 계산기**(온도 스케일 없이는 N = 32,768에서 손실이 내려가지 않는 이유)
5. **두 인코더와 공유 임베딩 공간** · ResNet·ViT 이미지 인코더, Transformer 텍스트 인코더, 선형 투영, 학습 설정
6. **브라우저에서 학습하는 미니 CLIP** · 색 4 × 모양 4 세계에서 같은 손실로 학습, 학습 때 못 본 조합의 제로샷 정확도, 배치 크기의 영향
7. **제로샷 분류** · 텍스트 인코더 = 분류기 가중치를 만드는 하이퍼네트워크, 방금 학습한 모델로 색·모양·16종 **분류기 조립**
8. **프롬프트 엔지니어링과 앙상블** · "A photo of a {label}." +1.3%p, 틀 80개 앙상블 +3.5%p, 임베딩 평균 모형
9. **결과** · Visual N-Grams 대비 11.5 → 76.2%, 제로샷 vs ResNet-50 선형 프로브(27개 중 16개), 제로샷 = 4-shot, 선형 프로브
10. **분포 이동** · 같은 ImageNet 76.2%인 ResNet-101과 제로샷 CLIP의 ImageNetV2·R·Sketch·A·ObjectNet 비교(그림 13)
11. **한계와 사회적 영향** · MNIST 88%, 세기·세밀한 분류, 405년, FairFace 실험
12. **그 뒤** · DALL·E 2, Stable Diffusion, LLaVA, ALIGN, SigLIP, 정리와 퀴즈 8문항

## 영상 노트

- **[Yannic Kilcher의 CLIP 해설 따라가기](https://dev-jonghoonpark.github.io/how-clip-works/kilcher.html)** — 「[OpenAI CLIP: Connecting Text and Images (Paper Explained)](https://www.youtube.com/watch?v=T9XSU0pKX2E)」(48분)을 영상 순서대로 정리. 점수를 매길 수 있는 모델이면 제로샷 분류기가 된다는 설명, 분류 사전학습이 "개를 한 덩어리로 뭉치는" 이유, few-shot 곡선이 먼저 내려가는 이유(시뮬레이션), "바나나는 노랗다". 구간별 영상 링크와 퀴즈 6문항

데모 2·3은 8×8 픽셀 장난감 세계에서 도는 작은 모델이고, 실제 CLIP 가중치를 돌리지는 않습니다.

## 참고

- Radford et al. (2021), *Learning Transferable Visual Models From Natural Language Supervision*, ICML 2021 — [arXiv:2103.00020](https://arxiv.org/abs/2103.00020) · [코드](https://github.com/openai/CLIP)
- Zhang et al. (2020), *Contrastive Learning of Medical Visual Representations from Paired Images and Text* (ConVIRT) — [arXiv:2010.00747](https://arxiv.org/abs/2010.00747)
- Li et al. (2017), *Learning Visual N-Grams from Web Data* — [arXiv:1612.09161](https://arxiv.org/abs/1612.09161)
- Taori et al. (2020), *Measuring Robustness to Natural Distribution Shifts in Image Classification* — [arXiv:2007.00644](https://arxiv.org/abs/2007.00644)
- Jia et al. (2021), *ALIGN* — [arXiv:2102.05918](https://arxiv.org/abs/2102.05918)
- Zhai et al. (2023), *Sigmoid Loss for Language Image Pre-Training* (SigLIP) — [arXiv:2303.15343](https://arxiv.org/abs/2303.15343)

[how-ai-works](https://github.com/dev-jonghoonpark/how-ai-works) 시리즈의 일부입니다.
