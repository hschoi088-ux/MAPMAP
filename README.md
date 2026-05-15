<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>맞춤 영어 회화 커리큘럼 추천</title>
  
  <script src="https://cdn.tailwindcss.com"></script>
  <script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  
  <style>
    /* 스크롤바 숨기기 및 애니메이션 효과 */
    .hide-scrollbar::-webkit-scrollbar { display: none; }
    .hide-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    .animate-fade-in { animation: fadeIn 0.4s ease-out forwards; }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(8px); }
      to { opacity: 1; transform: translateY(0); }
    }
  </style>
</head>
<body class="bg-gray-100">
  <div id="root"></div>

  <script type="text/babel">
    const { useState } = React;

    // --- 데이터베이스 ---
    const COURSES = {
      x1: { id: 'x1', instructor: 'X 강사 (원어민)', title: '왕초보 탈출 발음 집중반', tags: ['원어민', '왕초보', '발음교정'], desc: '짧은 문장을 반복하며 원어민의 억양과 발음을 그대로 입에 붙여요.' },
      x2: { id: 'x2', instructor: 'X 강사 (원어민)', title: '100% 영어 몰입 일기', tags: ['원어민', '스토리', '듣기강화'], desc: '재미있는 영어 일기 스토리로 유용한 표현을 자연스럽게 익히고 귀를 뚫어요.' },
      y1: { id: 'y1', instructor: 'Y 강사 (한국인)', title: '실전 원어민 100문장', tags: ['초급', '실전표현', '즉시활용'], desc: '원어민이 매일 쓰는 빈출 표현 100가지를 바로 써먹을 수 있게 가르쳐 드려요.' },
      y2: { id: 'y2', instructor: 'Y 강사 (한국인)', title: '가성비 끝판왕 58 기본 동사', tags: ['초급', '응용력', '가성비'], desc: '58개의 기본 동사만으로 수만 가지 문장을 만들어내는 기적의 응용법!' },
      y3: { id: 'y3', instructor: 'Y 강사 (한국인)', title: '구문 확장 마스터', tags: ['중급', '문법응용', '장문만들기'], desc: '알고 있는 표현을 이리저리 조합하고 확장하며 길게 말하는 법을 배워요.' },
      y4: { id: 'y4', instructor: 'Y 강사 (한국인)', title: '한계 돌파 심화 회화', tags: ['고급', '도전적과제', '논리적표현'], desc: '더 높은 수준의 어휘와 복잡한 구조로 내 생각을 완벽하게 논리적으로 표현해요.' }
    };

    const COMPARISON_ROUNDS = {
      'x1_x2': [
        { type: '예문 비교', question: '어떤 난이도의 문장으로 시작하고 싶나요?', a: { icon: '🔊', title: '짧고 명확한 관용구', text: '"It\'s on me." (내가 쏠게!)' }, b: { icon: '🔊', title: '리얼 일상 슬랭', text: '"I was so zoned out today." (오늘 완전 멍 때렸어.)' } },
        { type: '학습 방식', question: '선호하는 훈련 방식은?', a: { icon: '📈', title: '입모양 집중 쉐도잉', text: '강사의 입모양과 리듬을 보며 똑같이 따라하는 훈련' }, b: { icon: '📖', title: '스토리텔링 몰입', text: '일기 스토리를 들으며 흐름 속에서 표현을 유추하는 훈련' } },
        { type: '미니 퀴즈', question: '내가 강사에게 더 바라는 점은?', a: { icon: '💬', title: '디테일한 발음 교정', text: '"R과 L 발음의 차이를 정확히 짚어주면 좋겠어요."' }, b: { icon: '🎯', title: '원어민 뉘앙스 파악', text: '"이 상황에서 원어민들이 진짜로 쓰는 뉘앙스를 알려주면 좋겠어요."' } }
      ],
      'y1_y2': [
        { type: '예문 비교', question: '어떤 스타일의 문장이 더 끌리나요?', a: { icon: '🔊', title: '상황별 리액션 통문장', text: '"You read my mind!" (내 마음을 딱 읽었네!)' }, b: { icon: '🔊', title: '기본 동사 활용 문장', text: '"I\'ll have it done." (그거 끝내 놓을게.)' } },
        { type: '학습 방식', question: '선호하는 학습 포커스는?', a: { icon: '📈', title: '실전 표현 100개 통암기', text: '상황이 닥쳤을 때 바로 튀어나오게 표현을 통째로 흡수하기' }, b: { icon: '🧠', title: '1개로 10개 만들기', text: '동사 하나를 가지고 이리저리 문장을 변형해보는 뼈대 만들기' } },
        { type: '미니 퀴즈', question: '나의 현재 목표와 더 가까운 것은?', a: { icon: '🎯', title: '당장 써먹기', text: '"당장 내일 외국인에게 써먹을 수 있는 생생한 표현이 급해요."' }, b: { icon: '📖', title: '응용력 기르기', text: '"단어 하나를 알더라도 여러 문장으로 응용하는 원리를 배우고 싶어요."' } }
      ],
      'y3_y4': [
        { type: '예문 비교', question: '도전해보고 싶은 문장의 수준은?', a: { icon: '🔊', title: '패턴을 활용한 장문', text: '"Not only did I finish it, but I also..." (끝냈을 뿐만 아니라 ~도 했어.)' }, b: { icon: '🔊', title: '논리적이고 세련된 주장', text: '"It boils down to the fact that..." (결국 ~라는 사실로 귀결됩니다.)' } },
        { type: '학습 방식', question: '어떤 능력을 더 키우고 싶나요?', a: { icon: '📈', title: '막힘없는 연결', text: '아는 단어들을 접속사와 구문으로 매끄럽고 유창하게 이어 말하기' }, b: { icon: '🧠', title: '깊이 있는 토론', text: '특정 주제에 대해 내 의견을 뒷받침하는 고급 어휘와 구조 익히기' } },
        { type: '미니 퀴즈', question: '내가 꿈꾸는 나의 영어 실력은?', a: { icon: '💬', title: '자연스러운 프리토킹', text: '"번역투에서 벗어나 내가 하고 싶은 말을 길고 매끄럽게 하는 것"' }, b: { icon: '🎯', title: '네이티브급 토론', text: '"비즈니스 미팅이나 깊은 토론에서도 밀리지 않는 네이티브 수준의 구사력"' } }
      ]
    };

    function App() {
      const [step, setStep] = useState(0); 
      const [answers, setAnswers] = useState({ purpose: '', exp: '', level: '', style: '' });
      const [candidates, setCandidates] = useState([]); 
      const [scores, setScores] = useState([0, 0]); 
      const [compareRoundIdx, setCompareRoundIdx] = useState(0); 
      const [pairKey, setPairKey] = useState(''); 

      const handleAnswer = (key, val) => {
        const newAnswers = { ...answers, [key]: val };
        setAnswers(newAnswers);

        if (step < 4) {
          setStep(step + 1);
        } else {
          let cand = [];
          let keyStr = '';

          if (val === 'X') {
            cand = [COURSES.x1, COURSES.x2];
            keyStr = 'x1_x2';
          } else {
            if (newAnswers.level === 'A' || newAnswers.level === 'B') {
              cand = [COURSES.y1, COURSES.y2];
              keyStr = 'y1_y2';
            } else {
              cand = [COURSES.y3, COURSES.y4];
              keyStr = 'y3_y4';
            }
          }
          setCandidates(cand);
          setPairKey(keyStr);
          setStep(5);
        }
      };

      const handleCompareSelect = (candIndex) => {
        const newScores = [...scores];
        newScores[candIndex] += 1;
        setScores(newScores);

        if (compareRoundIdx < 2) {
          setCompareRoundIdx(compareRoundIdx + 1);
          setStep(step + 1);
        } else {
          setStep(9);
        }
      };

      const resetTest = () => {
        setStep(0);
        setAnswers({ purpose: '', exp: '', level: '', style: '' });
        setCandidates([]);
        setScores([0, 0]);
        setCompareRoundIdx(0);
        setPairKey('');
      };

      return (
        <div className="min-h-screen flex items-center justify-center p-4 sm:p-0">
          <div className="w-full max-w-md h-[850px] max-h-screen bg-white sm:rounded-[2.5rem] sm:shadow-2xl overflow-hidden relative flex flex-col border-4 border-gray-900">
            
            {/* 상단 프로그레스 바 */}
            {(step > 0 && step < 5) && (
              <div className="h-1.5 w-full bg-gray-100">
                <div className="h-full bg-indigo-600 transition-all duration-300" style={{ width: `${(step / 4) * 100}%` }}></div>
              </div>
            )}
            {(step > 5 && step < 9) && (
              <div className="h-1.5 w-full bg-gray-100">
                <div className="h-full bg-purple-500 transition-all duration-300" style={{ width: `${((step - 5) / 3) * 100}%` }}></div>
              </div>
            )}

            <div className="flex-grow overflow-hidden relative">
              {/* 시작 화면 */}
              {step === 0 && (
                <div className="flex flex-col items-center justify-center h-full px-6 text-center animate-fade-in">
                  <div className="text-5xl mb-6">✨</div>
                  <h1 className="text-3xl font-extrabold text-gray-900 mb-4 tracking-tight">실패 없는<br/>나만의 커리큘럼 찾기</h1>
                  <p className="text-gray-500 mb-10 leading-relaxed">목적, 경험, 수준을 입체적으로 분석하여<br/>나에게 가장 완벽한 핏(Fit)을 찾아드립니다.</p>
                  <button onClick={() => setStep(1)} className="w-full bg-indigo-600 text-white text-lg font-bold py-4 rounded-2xl hover:bg-indigo-700 transition shadow-lg shadow-indigo-200">테스트 시작하기</button>
                </div>
              )}
              
              {/* 질문 1 */}
              {step === 1 && (
                <div className="flex flex-col h-full px-6 pt-10 animate-fade-in">
                  <div className="text-sm font-bold text-indigo-600 mb-2">Step 1 / 4</div>
                  <h2 className="text-2xl font-bold text-gray-900 mb-8 leading-tight">영어 공부를 시작하려는<br/>결정적인 이유는 무엇인가요?</h2>
                  <div className="space-y-3">
                    <button onClick={() => handleAnswer('purpose', 'hobby')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">가벼운 일상 대화, 여행, 자막 없이 미드 시청 등 취미와 자기계발을 위해서요.</span></button>
                    <button onClick={() => handleAnswer('purpose', 'practical')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">외국인 친구 사귀기, 해외 취업/유학, 비즈니스 등 확실한 실전 소통이 필요해요.</span></button>
                  </div>
                </div>
              )}

              {/* 질문 2 */}
              {step === 2 && (
                <div className="flex flex-col h-full px-6 pt-10 animate-fade-in">
                  <div className="text-sm font-bold text-indigo-600 mb-2">Step 2 / 4</div>
                  <h2 className="text-2xl font-bold text-gray-900 mb-2 leading-tight">나의 이전 영어 공부 경험은<br/>어떤 편인가요?</h2>
                  <p className="text-gray-500 text-sm mb-8">나의 학습 성향을 파악합니다.</p>
                  <div className="space-y-3">
                    <button onClick={() => handleAnswer('exp', 'easy')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">꾸준히 해본 적이 없거나 매번 작심삼일이었어요. 쉽고 흥미를 유발하는 방식이 좋아요.</span></button>
                    <button onClick={() => handleAnswer('exp', 'system')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">나름 꾸준히 해봤지만 실력이 늘지 않아 정체기예요. 체계적이고 구체적인 방법론이 필요해요.</span></button>
                  </div>
                </div>
              )}

              {/* 질문 3 */}
              {step === 3 && (
                <div className="flex flex-col h-full px-6 pt-10 animate-fade-in">
                  <div className="text-sm font-bold text-indigo-600 mb-2">Step 3 / 4</div>
                  <h2 className="text-2xl font-bold text-gray-900 mb-2 leading-tight">현재 나의 영어 실력을<br/>가장 잘 설명한 문장은?</h2>
                  <p className="text-gray-500 text-sm mb-8">솔직하게 선택할수록 추천이 정확해집니다.</p>
                  <div className="space-y-3">
                    <button onClick={() => handleAnswer('level', 'A')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">영어 회화를 전혀 공부해본 적이 없어요. (왕초보)</span></button>
                    <button onClick={() => handleAnswer('level', 'B')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">영어로 말할 때 아는 단어 몇 개 정돈 던질 수 있어요. (초급)</span></button>
                    <button onClick={() => handleAnswer('level', 'C')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">말은 할 수 있지만, 문장이 번역투고 딱딱해서 아쉬워요. (중급)</span></button>
                    <button onClick={() => handleAnswer('level', 'D')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">기본적인 의사소통은 하지만 고급 표현과 뉘앙스까지 터득하고 싶어요. (고급)</span></button>
                  </div>
                </div>
              )}

              {/* 질문 4 */}
              {step === 4 && (
                <div className="flex flex-col h-full px-6 pt-10 animate-fade-in">
                  <div className="text-sm font-bold text-indigo-600 mb-2">Step 4 / 4</div>
                  <h2 className="text-2xl font-bold text-gray-900 mb-8 leading-tight">어떤 스타일의 강사에게<br/>배우고 싶나요?</h2>
                  <div className="space-y-3">
                    <button onClick={() => handleAnswer('style', 'X')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">원어민의 생생한 뉘앙스와 발음을 느낌 그대로 스펀지처럼 흡수하고 싶어요.</span></button>
                    <button onClick={() => handleAnswer('style', 'Y')} className="w-full text-left bg-white border-2 border-gray-100 p-5 rounded-2xl hover:border-indigo-600 hover:bg-indigo-50 transition-all"><span className="font-semibold text-gray-800 leading-snug">한국인 강사에게 문장 구조와 원리를 꼼꼼하고 체계적으로 배우고 싶어요.</span></button>
                  </div>
                </div>
              )}

              {/* 중간 전환 화면 */}
              {step === 5 && (
                <div className="flex flex-col items-center justify-center h-full px-6 text-center animate-fade-in bg-indigo-600 text-white">
                  <div className="text-5xl mb-6 opacity-90">🔍</div>
                  <h2 className="text-2xl font-bold mb-4 leading-tight">분석 완료!<br/>당신에게 딱 맞는<br/>2개의 커리큘럼을 찾았습니다.</h2>
                  <p className="text-indigo-100 mb-10 text-sm">이제부터 3번의 블라인드 비교 테스트를 통해<br/>최종 1개의 인생 강의를 결정합니다.</p>
                  <button onClick={() => setStep(6)} className="w-full bg-white text-indigo-600 text-lg font-bold py-4 rounded-2xl hover:bg-gray-50 transition">비교 테스트 시작 ➡️</button>
                </div>
              )}
              
              {/* 3라운드 비교 테스트 */}
              {(step === 6 || step === 7 || step === 8) && (
                <div className="flex flex-col h-full px-6 pt-8 pb-6 animate-fade-in bg-gray-50">
                  <div className="text-center mb-6">
                    <div className="inline-block bg-indigo-100 text-indigo-800 px-3 py-1 rounded-full text-xs font-bold mb-3 tracking-wide">
                      ROUND {compareRoundIdx + 1} / 3 : {COMPARISON_ROUNDS[pairKey][compareRoundIdx].type}
                    </div>
                    <h2 className="text-xl font-bold text-gray-900 leading-tight">
                      {COMPARISON_ROUNDS[pairKey][compareRoundIdx].question}
                    </h2>
                  </div>
                  <div className="flex-grow flex flex-col gap-4">
                    <button onClick={() => handleCompareSelect(0)} className="flex-1 bg-white border-2 border-gray-100 rounded-3xl p-6 text-left hover:border-indigo-500 hover:shadow-md transition-all flex flex-col justify-center">
                      <div className="text-2xl mb-3">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].a.icon}</div>
                      <h3 className="font-bold text-gray-900 text-lg mb-2">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].a.title}</h3>
                      <p className="text-gray-500 text-sm leading-relaxed">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].a.text}</p>
                    </button>
                    <div className="flex items-center justify-center -my-2 relative z-10 pointer-events-none">
                      <span className="bg-gray-900 text-white text-xs font-bold px-3 py-1 rounded-full shadow-sm">VS</span>
                    </div>
                    <button onClick={() => handleCompareSelect(1)} className="flex-1 bg-white border-2 border-gray-100 rounded-3xl p-6 text-left hover:border-purple-500 hover:shadow-md transition-all flex flex-col justify-center">
                      <div className="text-2xl mb-3">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].b.icon}</div>
                      <h3 className="font-bold text-gray-900 text-lg mb-2">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].b.title}</h3>
                      <p className="text-gray-500 text-sm leading-relaxed">{COMPARISON_ROUNDS[pairKey][compareRoundIdx].b.text}</p>
                    </button>
                  </div>
                </div>
              )}
              
              {/* 최종 결과 화면 */}
              {step === 9 && (
                <div className="flex flex-col items-center h-full px-6 pt-12 pb-8 animate-fade-in overflow-y-auto hide-scrollbar bg-white">
                  <div className="text-5xl mb-4">✅</div>
                  <h2 className="text-2xl font-bold text-gray-900 text-center mb-2">찾았다!<br/>당신의 포텐을 터뜨려줄 강의</h2>
                  <p className="text-gray-500 text-center mb-8 text-sm">3라운드의 취향 테스트를 거쳐<br/>당신에게 완벽히 동기화된 커리큘럼입니다.</p>

                  <div className="w-full bg-white rounded-3xl shadow-xl border border-gray-100 overflow-hidden mb-8">
                    <div className="bg-gray-900 p-6 text-white text-center">
                      <div className="text-gray-400 text-sm font-medium mb-1">{candidates[scores[0] > scores[1] ? 0 : 1].instructor}</div>
                      <h3 className="text-2xl font-bold mb-3">{candidates[scores[0] > scores[1] ? 0 : 1].title}</h3>
                      <div className="flex flex-wrap justify-center gap-2">
                        {candidates[scores[0] > scores[1] ? 0 : 1].tags.map(tag => (
                          <span key={tag} className="bg-white/20 px-3 py-1 rounded-full text-xs font-semibold backdrop-blur-sm">#{tag}</span>
                        ))}
                      </div>
                    </div>
                    <div className="p-6">
                      <p className="text-gray-700 text-center font-medium mb-6 leading-relaxed">{candidates[scores[0] > scores[1] ? 0 : 1].desc}</p>
                      <div className="bg-indigo-50 border border-indigo-100 rounded-2xl p-4 text-center">
                        <div className="text-indigo-800 font-bold mb-1">🎁 맞춤 매칭 혜택</div>
                        <div className="text-sm text-indigo-600">이 강의로 수강 시작 시 첫 달 50% 할인!</div>
                      </div>
                    </div>
                  </div>
                  <button className="w-full bg-indigo-600 text-white text-lg font-bold py-4 rounded-2xl hover:bg-indigo-700 transition shadow-lg shadow-indigo-200 mb-4 flex items-center justify-center gap-2">내 맞춤 커리큘럼 수강하기 ➡️</button>
                  <button onClick={resetTest} className="text-gray-400 text-sm font-medium hover:text-gray-600 underline underline-offset-4">테스트 다시하기</button>
                </div>
              )}
            </div>
          </div>
        </div>
      );
    }

    // React 앱 렌더링
    const root = ReactDOM.createRoot(document.getElementById('root'));
    root.render(<App />);
  </script>
</body>
</html>
