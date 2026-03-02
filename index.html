import React, { useState, useEffect, useRef } from 'react';
import { Mic, MicOff, Send, Volume2, BookOpen, AlertCircle, RefreshCw, VolumeX, Play, Sparkles, X } from 'lucide-react';

// --- API & Helper Functions ---
const apiKey = ""; // Provided by execution environment

const fetchWithRetry = async (url, options, retries = 5) => {
  let delay = 1000;
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(url, options);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return await response.json();
    } catch (error) {
      if (i === retries - 1) throw error;
      await new Promise(res => setTimeout(res, delay));
      delay *= 2;
    }
  }
};

const getAITutorResponse = async (userText) => {
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
  
  const systemInstruction = `You are an encouraging, friendly, and patient English language tutor for a native Hindi speaker who is a beginner at English.
Your goal is to help them learn English through daily conversation. 
The user will speak to you in Hindi or a mix of Hindi and broken English.
You must do the following:
1. Provide the correct English translation of what they tried to say.
2. Reply to them in simple, natural English to continue the conversation.
3. Provide a brief explanation of your reply in Hindi so they understand.
4. Pick out 1 to 3 new or important English vocabulary words from the context for them to learn.`;

  const payload = {
    contents: [{ parts: [{ text: `User said: "${userText}"` }] }],
    systemInstruction: { parts: [{ text: systemInstruction }] },
    generationConfig: {
      responseMimeType: "application/json",
      responseSchema: {
        type: "OBJECT",
        properties: {
          englishTranslation: { type: "STRING", description: "The correct English translation of the user's input." },
          aiResponseText: { type: "STRING", description: "Your conversational reply to the user in simple English." },
          hindiExplanation: { type: "STRING", description: "A friendly explanation in Hindi of your reply and any corrections." },
          vocabulary: {
            type: "ARRAY",
            items: {
              type: "OBJECT",
              properties: {
                word: { type: "STRING", description: "The English word" },
                meaningInHindi: { type: "STRING", description: "Meaning of the word in Hindi" },
                pronunciationHint: { type: "STRING", description: "How to pronounce the word (written in Hindi/Devanagari script)" }
              }
            }
          }
        },
        required: ["englishTranslation", "aiResponseText", "hindiExplanation", "vocabulary"]
      }
    }
  };

  const data = await fetchWithRetry(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
  });

  const responseText = data.candidates?.[0]?.content?.parts?.[0]?.text;
  if (!responseText) throw new Error("Invalid response from AI");
  return JSON.parse(responseText);
};

// NEW FEATURE 1: Generate a story using learned vocabulary
const getVocabStory = async (wordsArray) => {
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
  
  const systemInstruction = `You are an imaginative English tutor. Create a very short, engaging story (3-5 sentences) using the provided English vocabulary words. 
Write the story in simple English, then provide an accurate Hindi translation.`;

  const wordsList = wordsArray.map(w => w.word).join(', ');

  const payload = {
    contents: [{ parts: [{ text: `Create a story using these words: ${wordsList}` }] }],
    systemInstruction: { parts: [{ text: systemInstruction }] },
    generationConfig: {
      responseMimeType: "application/json",
      responseSchema: {
        type: "OBJECT",
        properties: {
          englishStory: { type: "STRING", description: "The short story in English." },
          hindiTranslation: { type: "STRING", description: "The Hindi translation of the story." }
        },
        required: ["englishStory", "hindiTranslation"]
      }
    }
  };

  const data = await fetchWithRetry(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
  });

  const responseText = data.candidates?.[0]?.content?.parts?.[0]?.text;
  if (!responseText) throw new Error("Invalid response from AI");
  return JSON.parse(responseText);
};

// NEW FEATURE 2: Suggest next replies based on context
const getReplySuggestions = async (conversationHistory) => {
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
  
  const systemInstruction = `You are a helpful language assistant for a Hindi speaker learning English. 
Based on the recent conversation, suggest 3 natural and contextually appropriate things the user could say next. 
Provide the suggestions primarily in Hindi so the user knows what they mean, along with the English translation.`;

  const payload = {
    contents: [{ parts: [{ text: `Recent conversation:\n${conversationHistory}\n\nSuggest 3 replies.` }] }],
    systemInstruction: { parts: [{ text: systemInstruction }] },
    generationConfig: {
      responseMimeType: "application/json",
      responseSchema: {
        type: "OBJECT",
        properties: {
          suggestions: {
            type: "ARRAY",
            items: {
              type: "OBJECT",
              properties: {
                hindi: { type: "STRING", description: "The suggested reply in Hindi" },
                english: { type: "STRING", description: "The English translation of the suggestion" }
              }
            }
          }
        },
        required: ["suggestions"]
      }
    }
  };

  const data = await fetchWithRetry(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
  });

  const responseText = data.candidates?.[0]?.content?.parts?.[0]?.text;
  if (!responseText) throw new Error("Invalid response from AI");
  return JSON.parse(responseText);
};

// Converts base64 PCM16 data from Gemini TTS to a playable WAV Blob
const createWavFile = (base64Data, sampleRate) => {
  const binaryString = atob(base64Data);
  const bytes = new Uint8Array(binaryString.length);
  for (let i = 0; i < binaryString.length; i++) {
    bytes[i] = binaryString.charCodeAt(i);
  }
  
  const buffer = new ArrayBuffer(44 + bytes.length);
  const view = new DataView(buffer);

  const writeString = (view, offset, string) => {
    for (let i = 0; i < string.length; i++) {
      view.setUint8(offset + i, string.charCodeAt(i));
    }
  };

  writeString(view, 0, 'RIFF');
  view.setUint32(4, 36 + bytes.length, true);
  writeString(view, 8, 'WAVE');
  writeString(view, 12, 'fmt ');
  view.setUint32(16, 16, true);
  view.setUint16(20, 1, true);
  view.setUint16(22, 1, true); // 1 channel (mono)
  view.setUint32(24, sampleRate, true);
  view.setUint32(28, sampleRate * 2, true);
  view.setUint16(32, 2, true);
  view.setUint16(34, 16, true); // 16 bits per sample
  writeString(view, 36, 'data');
  view.setUint32(40, bytes.length, true);

  const dataView = new Uint8Array(buffer, 44);
  dataView.set(bytes);

  return new Blob([buffer], { type: 'audio/wav' });
};

const generateSpeech = async (text) => {
  const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-tts:generateContent?key=${apiKey}`;
  const payload = {
    contents: [{ parts: [{ text: text }] }],
    generationConfig: {
      responseModalities: ["AUDIO"],
      speechConfig: {
        voiceConfig: {
          prebuiltVoiceConfig: {
            voiceName: "Aoede" // A clear, pleasant female voice
          }
        }
      }
    }
  };

  const data = await fetchWithRetry(url, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(payload)
  });

  const inlineData = data.candidates?.[0]?.content?.parts?.[0]?.inlineData;
  if (!inlineData) throw new Error("No audio data returned");

  const mimeType = inlineData.mimeType;
  let sampleRate = 24000; // Default fallback
  const rateMatch = mimeType.match(/rate=(\d+)/);
  if (rateMatch) {
    sampleRate = parseInt(rateMatch[1], 10);
  }

  const audioBlob = createWavFile(inlineData.data, sampleRate);
  return URL.createObjectURL(audioBlob);
};

// --- Main Component ---
export default function App() {
  const [messages, setMessages] = useState([
    {
      id: 1,
      role: 'ai',
      text: "Hello! I am your English Tutor. You can speak to me in Hindi, and I will help you learn English. Press the microphone button to start!",
      hindiExplanation: "नमस्ते! मैं आपका इंग्लिश ट्यूटर हूँ। आप मुझसे हिंदी में बात कर सकते हैं, और मैं आपको अंग्रेजी सीखने में मदद करूंगा। शुरू करने के लिए माइक बटन दबाएं!",
      vocabulary: []
    }
  ]);
  const [inputText, setInputText] = useState('');
  const [isListening, setIsListening] = useState(false);
  const [isLoading, setIsLoading] = useState(false);
  const [errorMsg, setErrorMsg] = useState('');
  const [activeAudioId, setActiveAudioId] = useState(null);
  
  // New State variables for features
  const [suggestions, setSuggestions] = useState([]);
  const [isGeneratingSuggestions, setIsGeneratingSuggestions] = useState(false);
  const [vocabStoryData, setVocabStoryData] = useState(null);
  const [isGeneratingStory, setIsGeneratingStory] = useState(false);
  const [showStoryModal, setShowStoryModal] = useState(false);
  
  const recognitionRef = useRef(null);
  const audioRef = useRef(new Audio());
  const messagesEndRef = useRef(null);

  // Initialize Speech Recognition
  useEffect(() => {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    if (SpeechRecognition) {
      recognitionRef.current = new SpeechRecognition();
      recognitionRef.current.continuous = false;
      recognitionRef.current.interimResults = false;
      recognitionRef.current.lang = 'hi-IN'; // Listen for Hindi

      recognitionRef.current.onstart = () => {
        setIsListening(true);
        setErrorMsg('');
      };

      recognitionRef.current.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        setInputText(transcript);
        handleSend(transcript);
      };

      recognitionRef.current.onerror = (event) => {
        setIsListening(false);
        if (event.error !== 'no-speech') {
          setErrorMsg('Microphone error: ' + event.error + '. Please try typing instead.');
        }
      };

      recognitionRef.current.onend = () => {
        setIsListening(false);
      };
    } else {
      setErrorMsg('Speech recognition is not supported in this browser. You can still type your messages.');
    }

    return () => {
      if (recognitionRef.current) recognitionRef.current.stop();
      audioRef.current.pause();
    };
  }, []);

  // Auto-scroll to bottom of chat
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages, isLoading, suggestions]);

  // Handle Audio Playback
  useEffect(() => {
    const audioEl = audioRef.current;
    const handleAudioEnd = () => setActiveAudioId(null);
    audioEl.addEventListener('ended', handleAudioEnd);
    return () => audioEl.removeEventListener('ended', handleAudioEnd);
  }, []);

  const toggleListen = () => {
    if (isListening) {
      recognitionRef.current?.stop();
    } else {
      setInputText('');
      recognitionRef.current?.start();
    }
  };

  const playAudio = async (text, messageId) => {
    try {
      if (activeAudioId === messageId) {
        audioRef.current.pause();
        setActiveAudioId(null);
        return;
      }

      const message = messages.find(m => m.id === messageId);
      let audioUrl = message?.audioUrl;

      if (!audioUrl) {
        setErrorMsg('');
        audioUrl = await generateSpeech(text);
        
        setMessages(prev => prev.map(m => m.id === messageId ? { ...m, audioUrl } : m));
      }

      audioRef.current.src = audioUrl;
      audioRef.current.play();
      setActiveAudioId(messageId);
    } catch (error) {
      console.error(error);
      setErrorMsg("Failed to generate voice. Please try again.");
      setActiveAudioId(null);
    }
  };

  const handleSend = async (textToProcess) => {
    const text = typeof textToProcess === 'string' ? textToProcess : inputText;
    if (!text.trim()) return;

    const newUserMsg = { id: Date.now(), role: 'user', text: text };
    setMessages(prev => [...prev, newUserMsg]);
    setInputText('');
    setSuggestions([]); // Clear suggestions when sending a new message
    setIsLoading(true);
    setErrorMsg('');

    try {
      const aiData = await getAITutorResponse(text);
      
      const newAiMsg = {
        id: Date.now() + 1,
        role: 'ai',
        text: aiData.aiResponseText,
        translation: aiData.englishTranslation,
        hindiExplanation: aiData.hindiExplanation,
        vocabulary: aiData.vocabulary || []
      };

      setMessages(prev => [...prev, newAiMsg]);
      
      // Automatically play the response so the user hears native pronunciation immediately
      playAudio(newAiMsg.text, newAiMsg.id);

    } catch (error) {
      console.error(error);
      setErrorMsg("I had trouble connecting. Please check your internet and try again.");
    } finally {
      setIsLoading(false);
    }
  };

  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleSend();
    }
  };

  // Feature Handlers
  const handleGenerateStory = async () => {
    if (allVocabulary.length === 0) return;
    setIsGeneratingStory(true);
    setShowStoryModal(true);
    setErrorMsg('');
    try {
      // Pick up to 10 recent words to keep the story short
      const wordsToUse = allVocabulary.slice(-10);
      const story = await getVocabStory(wordsToUse);
      setVocabStoryData(story);
    } catch (error) {
      console.error(error);
      setErrorMsg("Failed to generate story. Please try again.");
      setShowStoryModal(false);
    } finally {
      setIsGeneratingStory(false);
    }
  };

  const handleGetSuggestions = async () => {
    setIsGeneratingSuggestions(true);
    try {
      const recentHistory = messages.slice(-4).map(m => `${m.role === 'ai' ? 'Tutor' : 'Student'}: ${m.text}`).join('\n');
      const data = await getReplySuggestions(recentHistory);
      setSuggestions(data.suggestions || []);
    } catch (error) {
      console.error(error);
      setErrorMsg("Failed to get suggestions.");
    } finally {
      setIsGeneratingSuggestions(false);
    }
  };

  // Collect all vocabulary learned so far
  const allVocabulary = messages.reduce((acc, msg) => {
    if (msg.vocabulary && msg.vocabulary.length > 0) {
      acc.push(...msg.vocabulary);
    }
    return acc;
  }, []);

  return (
    <div className="flex flex-col md:flex-row h-screen bg-slate-50 font-sans text-slate-800 relative overflow-hidden">
      
      {/* Story Modal Overlay */}
      {showStoryModal && (
        <div className="absolute inset-0 z-50 flex items-center justify-center bg-slate-900/40 backdrop-blur-sm p-4">
          <div className="bg-white rounded-2xl shadow-2xl w-full max-w-lg overflow-hidden flex flex-col max-h-[90vh]">
            <div className="bg-amber-500 text-white p-4 flex justify-between items-center">
              <h3 className="font-bold text-lg flex items-center">
                <Sparkles className="w-5 h-5 mr-2" />
                Your Custom Story
              </h3>
              <button onClick={() => setShowStoryModal(false)} className="hover:bg-amber-600 p-1 rounded-full transition-colors">
                <X className="w-5 h-5" />
              </button>
            </div>
            <div className="p-6 overflow-y-auto">
              {isGeneratingStory ? (
                <div className="flex flex-col items-center justify-center py-10 space-y-4">
                  <RefreshCw className="w-8 h-8 text-amber-500 animate-spin" />
                  <p className="text-slate-500 font-medium">Writing a magical story using your words...</p>
                </div>
              ) : vocabStoryData ? (
                <div className="space-y-6">
                  <div>
                    <h4 className="text-sm font-bold text-slate-400 uppercase tracking-wider mb-2">English</h4>
                    <p className="text-lg text-slate-800 leading-relaxed font-medium">
                      {vocabStoryData.englishStory}
                    </p>
                  </div>
                  <div className="border-t border-slate-100 pt-4">
                    <h4 className="text-sm font-bold text-slate-400 uppercase tracking-wider mb-2">Hindi Translation</h4>
                    <p className="text-base text-slate-600 leading-relaxed">
                      {vocabStoryData.hindiTranslation}
                    </p>
                  </div>
                </div>
              ) : null}
            </div>
          </div>
        </div>
      )}

      {/* Main Chat Area */}
      <div className="flex-1 flex flex-col h-full border-r border-slate-200 shadow-sm relative">
        {/* Header */}
        <header className="bg-indigo-600 text-white p-4 flex items-center shadow-md z-10">
          <BookOpen className="w-6 h-6 mr-3" />
          <h1 className="text-xl font-bold tracking-wide">Namaste English Tutor</h1>
        </header>

        {/* Error Banner */}
        {errorMsg && (
          <div className="bg-red-100 border-l-4 border-red-500 text-red-700 p-3 m-4 rounded flex items-center animate-pulse">
            <AlertCircle className="w-5 h-5 mr-2" />
            <p className="text-sm">{errorMsg}</p>
          </div>
        )}

        {/* Chat History */}
        <div className="flex-1 overflow-y-auto p-4 space-y-6 pb-40">
          {messages.map((msg) => (
            <div key={msg.id} className={`flex flex-col ${msg.role === 'user' ? 'items-end' : 'items-start'}`}>
              
              {/* User Message Bubble */}
              {msg.role === 'user' && (
                <div className="bg-indigo-100 text-indigo-900 rounded-2xl rounded-tr-sm px-5 py-3 max-w-[85%] shadow-sm">
                  <p className="text-lg">{msg.text}</p>
                </div>
              )}

              {/* AI Message Bubble */}
              {msg.role === 'ai' && (
                <div className="w-full max-w-[90%] md:max-w-[80%] bg-white rounded-2xl rounded-tl-sm shadow-md border border-slate-100 overflow-hidden">
                  
                  {/* If there was a translation from user's Hindi */}
                  {msg.translation && (
                    <div className="bg-slate-50 border-b border-slate-100 px-4 py-2 flex items-start gap-2">
                      <span className="text-xs font-semibold text-slate-400 uppercase tracking-wider mt-1">You meant:</span>
                      <p className="text-slate-600 font-medium italic">"{msg.translation}"</p>
                    </div>
                  )}

                  {/* AI English Response */}
                  <div className="p-4 bg-indigo-50/30">
                    <div className="flex items-start justify-between">
                      <p className="text-xl text-indigo-950 font-medium leading-relaxed">{msg.text}</p>
                      
                      {/* Play Audio Button */}
                      <button 
                        onClick={() => playAudio(msg.text, msg.id)}
                        className={`ml-3 p-2 rounded-full shrink-0 transition-all ${
                          activeAudioId === msg.id 
                            ? 'bg-indigo-600 text-white shadow-lg animate-pulse' 
                            : 'bg-white text-indigo-600 hover:bg-indigo-100 shadow-sm border border-indigo-100'
                        }`}
                        title="Hear native pronunciation"
                      >
                        {activeAudioId === msg.id ? <VolumeX className="w-5 h-5" /> : <Volume2 className="w-5 h-5" />}
                      </button>
                    </div>
                  </div>

                  {/* Hindi Explanation */}
                  <div className="px-4 py-3 bg-white border-t border-slate-100">
                    <p className="text-slate-500 text-sm leading-relaxed">{msg.hindiExplanation}</p>
                  </div>

                  {/* Inline Vocabulary Cards */}
                  {msg.vocabulary && msg.vocabulary.length > 0 && (
                    <div className="px-4 py-3 bg-amber-50/50 border-t border-amber-100/50">
                      <p className="text-xs font-bold text-amber-700 uppercase tracking-wider mb-2">New Words (नये शब्द):</p>
                      <div className="flex flex-wrap gap-2">
                        {msg.vocabulary.map((v, idx) => (
                          <div key={idx} className="bg-white border border-amber-200 rounded-md px-3 py-1.5 text-sm shadow-sm">
                            <span className="font-bold text-slate-800">{v.word}</span>
                            <span className="text-slate-400 mx-1">-</span>
                            <span className="text-slate-600">{v.meaningInHindi}</span>
                          </div>
                        ))}
                      </div>
                    </div>
                  )}
                </div>
              )}
            </div>
          ))}
          
          {isLoading && (
            <div className="flex items-start">
              <div className="bg-white border border-slate-200 rounded-2xl rounded-tl-sm px-5 py-4 shadow-sm flex items-center gap-2">
                <div className="w-2 h-2 bg-indigo-400 rounded-full animate-bounce"></div>
                <div className="w-2 h-2 bg-indigo-400 rounded-full animate-bounce" style={{ animationDelay: '0.2s' }}></div>
                <div className="w-2 h-2 bg-indigo-400 rounded-full animate-bounce" style={{ animationDelay: '0.4s' }}></div>
              </div>
            </div>
          )}
          <div ref={messagesEndRef} />
        </div>

        {/* Bottom Area: Suggestions & Input */}
        <div className="absolute bottom-0 left-0 right-0 bg-white border-t border-slate-200 px-3 pb-3 pt-2 shadow-[0_-4px_6px_-1px_rgba(0,0,0,0.05)]">
          <div className="max-w-4xl mx-auto flex flex-col gap-2">
            
            {/* Smart Suggestions Row */}
            {!isLoading && messages.length > 1 && suggestions.length === 0 && !isGeneratingSuggestions && (
              <div className="flex justify-start">
                <button 
                  onClick={handleGetSuggestions}
                  className="flex items-center text-xs font-medium bg-indigo-50 text-indigo-600 hover:bg-indigo-100 px-3 py-1.5 rounded-full transition-colors border border-indigo-100"
                >
                  <Sparkles className="w-3.5 h-3.5 mr-1" />
                  Need a hint? (क्या बोलूं?)
                </button>
              </div>
            )}

            {isGeneratingSuggestions && (
              <div className="flex justify-start">
                <span className="text-xs text-slate-400 flex items-center">
                  <RefreshCw className="w-3 h-3 mr-1 animate-spin" /> Thinking of ideas...
                </span>
              </div>
            )}

            {suggestions.length > 0 && (
              <div className="flex flex-wrap gap-2 mb-1">
                {suggestions.map((s, i) => (
                  <button 
                    key={i}
                    onClick={() => setInputText(s.hindi)}
                    className="flex flex-col items-start text-left bg-indigo-50 border border-indigo-200 hover:bg-indigo-100 rounded-lg px-3 py-2 transition-colors max-w-sm"
                    title={s.english}
                  >
                    <span className="text-sm font-medium text-indigo-900 flex items-center">
                      <Sparkles className="w-3 h-3 mr-1 text-indigo-500" />
                      {s.hindi}
                    </span>
                    <span className="text-xs text-indigo-500">{s.english}</span>
                  </button>
                ))}
              </div>
            )}

            <div className="flex items-end gap-2">
              {/* Voice Input Button */}
              <button
                onClick={toggleListen}
                className={`p-4 rounded-full flex-shrink-0 transition-all transform hover:scale-105 ${
                  isListening 
                    ? 'bg-red-500 text-white shadow-[0_0_15px_rgba(239,68,68,0.5)] animate-pulse' 
                    : 'bg-indigo-100 text-indigo-600 hover:bg-indigo-200'
                }`}
                title="Click to speak in Hindi"
              >
                {isListening ? <Mic className="w-6 h-6" /> : <MicOff className="w-6 h-6" />}
              </button>

              {/* Text Input Fallback */}
              <div className="flex-1 bg-slate-100 rounded-2xl flex items-center px-4 py-2 focus-within:ring-2 focus-within:ring-indigo-400 transition-all">
                <input
                  type="text"
                  value={inputText}
                  onChange={(e) => setInputText(e.target.value)}
                  onKeyPress={handleKeyPress}
                  placeholder={isListening ? "Listening... (सुन रहा हूँ...)" : "Type or speak in Hindi..."}
                  className="w-full bg-transparent border-none focus:outline-none py-2 text-slate-700 placeholder-slate-400"
                  disabled={isListening}
                />
              </div>

              {/* Send Button */}
              <button
                onClick={handleSend}
                disabled={!inputText.trim() || isLoading}
                className="p-4 bg-indigo-600 text-white rounded-full flex-shrink-0 hover:bg-indigo-700 disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
              >
                <Send className="w-5 h-5 ml-1" />
              </button>
            </div>
            <p className="text-center text-xs text-slate-400">
              Tip: Press the microphone and speak naturally in Hindi.
            </p>
          </div>
        </div>
      </div>

      {/* Side Panel: Vocabulary Bank (Desktop) */}
      <div className="hidden md:flex w-80 bg-white flex-col shadow-[-4px_0_15px_-3px_rgba(0,0,0,0.05)] z-20">
        <div className="p-4 bg-amber-50 border-b border-amber-100 flex flex-col gap-3">
          <div className="flex items-center justify-between">
            <h2 className="font-bold text-amber-900 flex items-center">
              <BookOpen className="w-5 h-5 mr-2" />
              My Dictionary
            </h2>
            <span className="bg-amber-200 text-amber-800 text-xs font-bold px-2 py-1 rounded-full">
              {allVocabulary.length} Words
            </span>
          </div>
          {allVocabulary.length > 0 && (
            <button 
              onClick={handleGenerateStory}
              disabled={isGeneratingStory}
              className="w-full py-2 bg-amber-500 hover:bg-amber-600 text-white text-sm font-bold rounded-lg shadow-sm transition-colors flex items-center justify-center disabled:opacity-50"
            >
              {isGeneratingStory ? <RefreshCw className="w-4 h-4 mr-2 animate-spin" /> : <Sparkles className="w-4 h-4 mr-2" />}
              ✨ Story from Vocab
            </button>
          )}
        </div>
        
        <div className="flex-1 overflow-y-auto p-4 space-y-3">
          {allVocabulary.length === 0 ? (
            <div className="text-center text-slate-400 mt-10">
              <p>Start chatting to collect new words!</p>
              <p className="text-xs mt-2">नए शब्द यहाँ दिखाई देंगे।</p>
            </div>
          ) : (
            allVocabulary.map((v, i) => (
              <div key={i} className="bg-white border border-slate-100 rounded-xl p-3 shadow-sm hover:shadow-md transition-shadow">
                <div className="flex justify-between items-start mb-1">
                  <h3 className="font-bold text-lg text-indigo-900">{v.word}</h3>
                  <button 
                    onClick={() => playAudio(v.word, `vocab-${i}`)}
                    className="text-slate-400 hover:text-indigo-600 p-1"
                    title="Pronounce word"
                  >
                    <Volume2 className="w-4 h-4" />
                  </button>
                </div>
                <p className="text-slate-600 text-sm font-medium">{v.meaningInHindi}</p>
                {v.pronunciationHint && (
                  <p className="text-slate-400 text-xs mt-1">🗣️ {v.pronunciationHint}</p>
                )}
              </div>
            ))
          )}
        </div>
      </div>

    </div>
  );
}
