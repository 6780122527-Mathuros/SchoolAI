<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>HappySchool</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <style>
    body {
      font-family: 'Sarabun', sans-serif;
      background: linear-gradient(135deg, #f4eaff 0%, #d3ecfd 100%);
      color: #444;
    }
  </style>
  <script type="importmap">
{
  "imports": {
    "@google/genai": "https://aistudiocdn.com/@google/genai@^1.30.0",
    "react/": "https://aistudiocdn.com/react@^19.2.0/",
    "react": "https://aistudiocdn.com/react@^19.2.0",
    "react-dom/": "https://aistudiocdn.com/react-dom@^19.2.0/",
    "lucide-react": "https://aistudiocdn.com/lucide-react@^0.555.0",
    "recharts": "https://aistudiocdn.com/recharts@^3.5.0",
    "@vitejs/plugin-react": "https://aistudiocdn.com/@vitejs/plugin-react@^5.1.1",
    "vite": "https://aistudiocdn.com/vite@^7.2.4"
  }
}
</script>
</head>
<body>
  <div id="root"></div>
  <script type="module" src="/index.tsx"></script>
</body>
import React, { useState } from 'react';
import { Role, User, Appointment } from './types';
import StudentDashboard from './components/StudentDashboard';
import TeacherDashboard from './components/TeacherDashboard';
import AdminDashboard from './components/AdminDashboard';

const App: React.FC = () => {
  const [role, setRole] = useState<Role>('student');
  const [currentUser, setCurrentUser] = useState<User>({
    id: '1',
    name: 'เด็กหญิงสดใส ใจดี',
    role: 'student',
    stars: 24,
  });

  const [appointments, setAppointments] = useState<Appointment[]>([
    { 
      id: '101', 
      studentName: 'เด็กหญิงสดใส ใจดี', 
      date: '2023-10-28 14:00', 
      reason: 'เครียดเรื่องสอบ อยากขอคำแนะนำค่ะ', 
      status: 'pending' 
    }
  ]);

  const handleRoleChange = (newRole: Role) => {
    setRole(newRole);
    if (newRole === 'student') {
        setCurrentUser({ ...currentUser, role: 'student', name: 'เด็กหญิงสดใส ใจดี' });
    } else if (newRole === 'teacher') {
        setCurrentUser({ ...currentUser, role: 'teacher', name: 'ครูสมศรี ที่รัก' });
    } else {
        setCurrentUser({ ...currentUser, role: 'admin', name: 'ผอ. ใจกว้าง' });
    }
  };

  const handleUpdateNickname = (nickname: string) => {
    setCurrentUser(prev => ({ ...prev, nickname }));
  };

  const handleApproveAppointment = (id: string, note: string) => {
    setAppointments(prev => prev.map(a => 
      a.id === id 
        ? { ...a, status: 'approved', teacherNote: note } 
        : a
    ));
  };

  return (
    <div className="min-h-screen pb-12 font-['Sarabun']">
      {/* Header */}
      <header className="text-center bg-[#fcecfb] border-b-2 border-[#e5d9f7] pt-[1.7em] pb-[0.3em]">
        <h1 className="text-[#a645ae] text-3xl font-bold mt-[1.5em] mb-[0.1em]">HappySchool</h1>
        <div className="text-[#a14f88] mb-4">ระบบติดตามอารมณ์และดาวเด็กดี</div>
      </header>

      {/* Navigation */}
      <nav className="text-center p-[1.1em] bg-[#f2f7fd]">
        <div className="flex justify-center flex-wrap gap-2">
          <button 
            onClick={() => handleRoleChange('student')}
            className={`text-[1.19em] border-none rounded-[11px] px-[2.2em] py-[0.8em] m-[0.4em] cursor-pointer transition-colors font-medium ${
              role === 'student' 
                ? 'bg-[#a645ae] text-white shadow-md' 
                : 'bg-[#e9dfff] text-[#86398e] hover:bg-[#e4e5ff]'
            }`}
          >
            👦 นักเรียน
          </button>
          <button 
            onClick={() => handleRoleChange('teacher')}
            className={`text-[1.19em] border-none rounded-[11px] px-[2.2em] py-[0.8em] m-[0.4em] cursor-pointer transition-colors font-medium ${
              role === 'teacher' 
                ? 'bg-[#a645ae] text-white shadow-md' 
                : 'bg-[#e9dfff] text-[#86398e] hover:bg-[#e4e5ff]'
            }`}
          >
            👩‍🏫 ครู
          </button>
          <button 
            onClick={() => handleRoleChange('admin')}
            className={`text-[1.19em] border-none rounded-[11px] px-[2.2em] py-[0.8em] m-[0.4em] cursor-pointer transition-colors font-medium ${
              role === 'admin' 
                ? 'bg-[#a645ae] text-white shadow-md' 
                : 'bg-[#e9dfff] text-[#86398e] hover:bg-[#e4e5ff]'
            }`}
          >
            🏫 ผู้บริหาร
          </button>
          <button 
             onClick={() => window.location.reload()}
             className="bg-[#a651b1] text-white border-none rounded-[9px] font-bold text-[1.07em] px-[2em] py-[0.7em] m-[0.4em] shadow-[0_1px_7px_#ede0fb40] hover:bg-[#e6c6f5] hover:text-[#640a73] ml-auto"
          >
            ออกจากระบบ
          </button>
        </div>
      </nav>

      {/* Main Content Section */}
      <section className="max-w-[930px] mx-auto my-[2em] bg-[#fffefe] rounded-[23px] p-[2em_2.2em] shadow-[0_4px_25px_#e4eaf4cc]">
        {role === 'student' && (
          <StudentDashboard 
            user={currentUser} 
            onUpdateStars={(amount) => setCurrentUser({...currentUser, stars: currentUser.stars + amount})}
            appointments={appointments.filter(a => a.studentName === currentUser.name)}
            onUpdateNickname={handleUpdateNickname}
          />
        )}
        {role === 'teacher' && (
          <TeacherDashboard 
            appointments={appointments}
            onApprove={handleApproveAppointment}
          />
        )}
        {role === 'admin' && <AdminDashboard />}
      </section>

      <footer className="text-center text-gray-400 text-sm mt-12 mb-4">
        © 2024 HappySchool System. Powered by Gemini API.
      </footer>
    </div>
  );
};

export default App;
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const rootElement = document.getElementById('root');
if (!rootElement) {
  throw new Error("Could not find root element to mount to");
}

const root = ReactDOM.createRoot(rootElement);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
export type Role = 'student' | 'teacher' | 'admin';

export interface User {
  id: string;
  name: string;
  nickname?: string;
  role: Role;
  stars: number;
}

export interface DiaryEntry {
  id: string;
  date: string;
  text: string;
  mood: string;
  aiResponse?: string;
}

export interface Appointment {
  id: string;
  studentName: string;
  date: string;
  reason: string;
  status: 'pending' | 'approved' | 'completed';
  teacherNote?: string;
}

export interface EmotionStat {
  name: string;
  value: number;
  color: string;
}

export const MOCK_STUDENTS: User[] = [
  { id: '1', name: 'น้องฟ้า (Fah)', role: 'student', stars: 12 },
  { id: '2', name: 'น้องวิน (Win)', role: 'student', stars: 8 },
  { id: '3', name: 'น้องแก้ม (Gam)', role: 'student', stars: 15 },
];
{
  "name": "HappySchool",
  "description": "A comprehensive student emotion tracking and reward system powered by Gemini API for mental health support and analytics.",
  "requestFramePermissions": []
}
import React from 'react';
import { PieChart, Pie, Cell, ResponsiveContainer, Tooltip, Legend, BarChart, Bar, XAxis, YAxis, CartesianGrid } from 'recharts';
import { EmotionStat } from '../types';
import { BarChart3, PieChart as PieIcon, GraduationCap, Briefcase } from 'lucide-react';

const AdminDashboard: React.FC = () => {
  // Mock Data Students
  const studentEmotionData: EmotionStat[] = [
    { name: 'Happy', value: 40, color: '#b1e5e0' },
    { name: 'Neutral', value: 25, color: '#ffe780' },
    { name: 'Sad', value: 15, color: '#a651b1' },
    { name: 'Anxious', value: 10, color: '#ffd7ef' },
    { name: 'Excited', value: 10, color: '#bfffa5' },
  ];

  // Mock Data Teachers
  const teacherEmotionData = [
    { name: 'Happy', value: 18, color: '#b1e5e0' },
    { name: 'Neutral', value: 12, color: '#ffe780' },
    { name: 'Tired', value: 5, color: '#a651b1' },
    { name: 'Excited', value: 14, color: '#bfffa5' },
    { name: 'Stressed', value: 6, color: '#ffd7ef' },
  ];

  const activityData = [
    { name: 'Mon', entries: 12 },
    { name: 'Tue', entries: 19 },
    { name: 'Wed', entries: 15 },
    { name: 'Thu', entries: 22 },
    { name: 'Fri', entries: 30 },
  ];

  return (
    <div className="space-y-6">
      <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] mb-[2em] shadow-[0_1px_18px_#e7e1fa60]">
        <h2 className="text-[#a645ae] text-2xl font-bold mt-0 letter-spacing-[.03em] mb-1">แผงควบคุมผู้บริหาร 🏫</h2>
        <p className="text-[#7193a6]">ภาพรวมสุขภาพจิตทั้งโรงเรียน</p>
      </div>

      {/* Row 1: Emotion Distribution (Student vs Teacher) */}
      <div className="grid md:grid-cols-2 gap-6">
        {/* Student Emotion */}
        <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60] flex flex-col items-center">
          <h3 className="text-[#7193a6] text-lg font-bold mb-2 w-full flex items-center gap-2">
            <GraduationCap className="text-[#a651b1]" /> สัดส่วนอารมณ์นักเรียน
          </h3>
          <div className="w-full h-64">
            <ResponsiveContainer width="100%" height="100%">
              <PieChart>
                <Pie
                  data={studentEmotionData}
                  cx="50%"
                  cy="50%"
                  innerRadius={60}
                  outerRadius={80}
                  paddingAngle={5}
                  dataKey="value"
                >
                  {studentEmotionData.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} stroke="none" />
                  ))}
                </Pie>
                <Tooltip 
                  contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
                />
                <Legend iconType="circle" />
              </PieChart>
            </ResponsiveContainer>
          </div>
          <p className="text-sm text-gray-500 text-center mt-2">นักเรียนส่วนใหญ่มีความสุขและผ่อนคลาย</p>
        </div>

        {/* Teacher Emotion */}
        <div className="bg-[#fffefe] border border-[#d3ecfd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60] flex flex-col items-center">
          <h3 className="text-[#7193a6] text-lg font-bold mb-2 w-full flex items-center gap-2">
            <Briefcase className="text-[#2c7a7b]" /> สถิติอารมณ์คุณครู
          </h3>
          <div className="w-full h-64">
            <ResponsiveContainer width="100%" height="100%">
              <BarChart data={teacherEmotionData} layout="vertical" margin={{ top: 5, right: 30, left: 20, bottom: 5 }}>
                <CartesianGrid strokeDasharray="3 3" horizontal={false} stroke="#e0e0e0" />
                <XAxis type="number" hide />
                <YAxis dataKey="name" type="category" width={70} tick={{fill: '#6b7280', fontSize: 12}} axisLine={false} tickLine={false} />
                <Tooltip 
                  cursor={{fill: '#f3f4f6'}}
                  contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
                />
                <Bar dataKey="value" radius={[0, 4, 4, 0]} barSize={20}>
                  {teacherEmotionData.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} />
                  ))}
                </Bar>
              </BarChart>
            </ResponsiveContainer>
          </div>
          <p className="text-sm text-gray-500 text-center mt-2">คุณครูมีความกระตือรือร้นสูงในการสอน</p>
        </div>
      </div>

      {/* Row 2: Weekly Activity */}
      <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
        <h3 className="text-[#7193a6] text-lg font-bold mb-6 w-full flex items-center gap-2">
          <BarChart3 className="text-[#a651b1]" /> การบันทึกไดอารี่รายสัปดาห์ (ทั้งโรงเรียน)
        </h3>
        <div className="w-full h-64">
          <ResponsiveContainer width="100%" height="100%">
            <BarChart data={activityData}>
              <CartesianGrid strokeDasharray="3 3" stroke="#e0e0e0" vertical={false} />
              <XAxis dataKey="name" axisLine={false} tickLine={false} tick={{fill: '#9ca3af'}} />
              <YAxis axisLine={false} tickLine={false} tick={{fill: '#9ca3af'}} />
              <Tooltip 
                cursor={{fill: '#f3f4f6'}}
                contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
              />
              <Bar dataKey="entries" fill="#a651b1" radius={[6, 6, 0, 0]} barSize={30} />
            </BarChart>
          </ResponsiveContainer>
        </div>
      </div>

      {/* Stats Cards */}
      <div className="grid grid-cols-3 gap-4">
        <div className="bg-[#e9dfff] rounded-2xl p-4 text-center">
          <h4 className="text-[#86398e] font-bold text-sm">นักเรียนทั้งหมด</h4>
          <p className="text-2xl font-bold text-[#640a73] mt-1">1,240</p>
        </div>
        <div className="bg-[#c6f6d5] rounded-2xl p-4 text-center">
           <h4 className="text-[#276749] font-bold text-sm">คุณครูทั้งหมด</h4>
           <p className="text-2xl font-bold text-[#22543d] mt-1">85</p>
        </div>
        <div className="bg-[#ffd7ef] rounded-2xl p-4 text-center">
           <h4 className="text-[#d13d84] font-bold text-sm">กลุ่มเสี่ยง (รวม)</h4>
           <p className="text-2xl font-bold text-[#9e2b63] mt-1">15</p>
        </div>
      </div>
    </div>
  );
};

export default AdminDashboard;
import React, { useState } from 'react';
import { User, DiaryEntry, Appointment } from '../types';
import { Star, Smile, Frown, Meh, Heart, CloudRain, Sparkles, Calendar, MessageCircle, PieChart as PieIcon, Edit2, Check, X } from 'lucide-react';
import { PieChart, Pie, Cell, ResponsiveContainer, Tooltip, Legend } from 'recharts';
import { getSupportiveAdvice } from '../services/geminiService';

interface StudentDashboardProps {
  user: User;
  onUpdateStars: (amount: number) => void;
  appointments: Appointment[];
  onUpdateNickname: (nickname: string) => void;
}

const StudentDashboard: React.FC<StudentDashboardProps> = ({ user, appointments, onUpdateNickname }) => {
  const [selectedMood, setSelectedMood] = useState<string>('Happy');
  const [diaryText, setDiaryText] = useState('');
  const [entries, setEntries] = useState<DiaryEntry[]>([]);
  const [isConsultingAi, setIsConsultingAi] = useState(false);
  
  // Nickname state
  const [isEditingName, setIsEditingName] = useState(false);
  const [nicknameInput, setNicknameInput] = useState(user.nickname || '');

  const moods = [
    { label: 'Happy', icon: <Smile className="w-6 h-6" /> },
    { label: 'Neutral', icon: <Meh className="w-6 h-6" /> },
    { label: 'Sad', icon: <Frown className="w-6 h-6" /> },
    { label: 'Excited', icon: <Heart className="w-6 h-6" /> },
    { label: 'Anxious', icon: <CloudRain className="w-6 h-6" /> },
  ];

  // Mock stats for the student
  const studentStats = [
    { name: 'Happy', value: 15, color: '#b1e5e0' },
    { name: 'Neutral', value: 8, color: '#ffe780' },
    { name: 'Excited', value: 6, color: '#bfffa5' },
    { name: 'Sad', value: 2, color: '#a651b1' },
  ];

  const handleDiarySubmit = async () => {
    if (!diaryText.trim()) return;

    setIsConsultingAi(true);
    let aiFeedback = '';
    
    try {
        aiFeedback = await getSupportiveAdvice(diaryText, selectedMood);
    } catch (e) {
        aiFeedback = "ไม่สามารถเชื่อมต่อกับ AI ได้ในขณะนี้";
    }

    const newEntry: DiaryEntry = {
      id: Date.now().toString(),
      date: new Date().toLocaleString('th-TH'),
      text: diaryText,
      mood: selectedMood,
      aiResponse: aiFeedback,
    };

    setEntries([newEntry, ...entries]);
    setDiaryText('');
    setIsConsultingAi(false);
  };

  const handleSaveNickname = () => {
    onUpdateNickname(nicknameInput);
    setIsEditingName(false);
  };

  return (
    <div className="space-y-6 animate-fade-in">
      {/* Header Stats */}
      <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] mb-[2em] shadow-[0_1px_18px_#e7e1fa60] flex justify-between items-center">
        <div>
          {isEditingName ? (
            <div className="flex items-center gap-2 mb-1">
              <span className="text-[#a645ae] text-2xl font-bold">สวัสดี, </span>
              <input
                type="text"
                value={nicknameInput}
                onChange={(e) => setNicknameInput(e.target.value)}
                placeholder="ใส่ชื่อเล่น..."
                className="p-1 px-3 rounded-lg border-2 border-[#a651b1] text-lg w-40 focus:outline-none text-[#a645ae] bg-white"
                autoFocus
                onKeyDown={(e) => e.key === 'Enter' && handleSaveNickname()}
              />
              <button onClick={handleSaveNickname} className="p-1.5 bg-[#b1e5e0] text-[#2c7a7b] rounded-md hover:bg-[#9adfd9] transition-colors">
                <Check className="w-5 h-5" />
              </button>
              <button onClick={() => setIsEditingName(false)} className="p-1.5 bg-[#ffd7ef] text-[#d13d84] rounded-md hover:bg-[#ffaad4] transition-colors">
                <X className="w-5 h-5" />
              </button>
            </div>
          ) : (
            <div className="flex items-center gap-2 group mb-1">
              <h2 className="text-[#a645ae] text-2xl font-bold mt-0 letter-spacing-[.03em]">
                สวัสดี, {user.nickname || user.name} 👋
              </h2>
              <button
                onClick={() => {
                  setNicknameInput(user.nickname || '');
                  setIsEditingName(true);
                }}
                className="opacity-0 group-hover:opacity-100 transition-opacity text-[#a651b1] hover:bg-[#fcecfb] p-1 rounded-md"
                title="แก้ไขชื่อเล่น"
              >
                <Edit2 className="w-4 h-4" />
              </button>
            </div>
          )}
          <p className="text-[#7193a6]">วันนี้รู้สึกอย่างไรบ้าง?</p>
        </div>
        <div className="bg-[#fffefe] px-4 py-2 rounded-2xl border border-[#d3ecfd] flex items-center gap-2">
          <Star className="w-6 h-6 text-[#ffe780] fill-[#ffe780]" />
          <span className="text-2xl font-bold text-[#a651b1]">{user.stars}</span>
        </div>
      </div>

      <div className="grid md:grid-cols-2 gap-6">
        {/* Mood Selector & Diary */}
        <div className="space-y-6">
          <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
            <h3 className="text-[#7193a6] text-lg font-semibold mb-4">เช็คอินอารมณ์</h3>
            <div className="flex flex-wrap gap-2">
              {moods.map((m) => (
                <button
                  key={m.label}
                  onClick={() => setSelectedMood(m.label)}
                  className={`flex-1 min-w-[60px] flex flex-col items-center justify-center p-3 rounded-2xl border-[1.3px] transition-all duration-200 ${
                    selectedMood === m.label 
                      ? 'bg-[#ffd7ef] border-[#bb5ecf] scale-105 shadow-md' 
                      : 'bg-[#e6f6ff] border-[#b6c5ee] hover:bg-[#f0f9ff]'
                  }`}
                >
                  <div className={selectedMood === m.label ? 'text-[#a645ae]' : 'text-[#7193a6]'}>
                      {m.icon}
                  </div>
                  <span className={`mt-1 text-xs font-medium ${selectedMood === m.label ? 'text-[#a645ae]' : 'text-[#7193a6]'}`}>{m.label}</span>
                </button>
              ))}
            </div>
          </div>

          <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
            <h3 className="text-[#7193a6] text-lg font-semibold mb-4">ไดอารี่ประจำวัน 📒</h3>
            <textarea
              value={diaryText}
              onChange={(e) => setDiaryText(e.target.value)}
              placeholder="วันนี้เจอเรื่องอะไรมาบ้าง เล่าให้ฟังหน่อยสิ..."
              className="w-full h-32 p-[0.7em] my-[0.13em] rounded-[7px] border-[1.25px] border-[#d3ecfd] bg-[#fff6f8] focus:border-[#a651b1] focus:outline-none resize-none text-gray-700 mb-4"
            />
            <button
              onClick={handleDiarySubmit}
              disabled={isConsultingAi || !diaryText.trim()}
              className="w-full bg-[#a651b1] text-white rounded-[9px] font-bold text-[1.07em] px-[2em] py-[0.7em] shadow-[0_1px_7px_#ede0fb40] hover:bg-[#e6c6f5] hover:text-[#640a73] transition-all flex items-center justify-center gap-2 disabled:opacity-70 disabled:cursor-not-allowed"
            >
              {isConsultingAi ? (
                <Sparkles className="w-5 h-5 animate-spin" />
              ) : (
                <Sparkles className="w-5 h-5" />
              )}
              {isConsultingAi ? 'AI กำลังคิด...' : 'บันทึก & ถาม AI'}
            </button>
          </div>
        </div>

        {/* Right Column: Chart, Appointments & History */}
        <div className="space-y-6">
           {/* Emotion Chart */}
           <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
             <h3 className="text-[#7193a6] text-lg font-semibold mb-4 flex items-center gap-2">
               <PieIcon className="w-5 h-5 text-[#a651b1]" /> สรุปอารมณ์เดือนนี้
             </h3>
             <div className="h-48 w-full">
               <ResponsiveContainer width="100%" height="100%">
                 <PieChart>
                   <Pie
                     data={studentStats}
                     cx="50%"
                     cy="50%"
                     innerRadius={40}
                     outerRadius={60}
                     paddingAngle={5}
                     dataKey="value"
                   >
                     {studentStats.map((entry, index) => (
                       <Cell key={`cell-${index}`} fill={entry.color} stroke="none" />
                     ))}
                   </Pie>
                   <Tooltip 
                     contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
                   />
                   <Legend iconType="circle" wrapperStyle={{ fontSize: '12px' }} />
                 </PieChart>
               </ResponsiveContainer>
             </div>
           </div>

           {/* Appointments Section */}
           <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
             <h3 className="text-[#7193a6] text-lg font-semibold mb-4 flex items-center gap-2">
               <Calendar className="w-5 h-5" /> นัดหมายกับครู
             </h3>
             {appointments.length === 0 ? (
                <p className="text-gray-400 text-sm text-center">ไม่มีการนัดหมายเร็วๆ นี้</p>
             ) : (
                <div className="space-y-3">
                  {appointments.map(appt => (
                    <div key={appt.id} className="bg-white p-3 rounded-xl border border-[#d3ecfd]">
                       <div className="flex justify-between items-center mb-2">
                         <span className="text-sm font-bold text-[#a645ae]">{appt.date}</span>
                         <span className={`text-[10px] px-2 py-0.5 rounded-full ${appt.status === 'approved' ? 'bg-[#d9fff5] text-[#399d2f]' : 'bg-[#fff5d6] text-[#b38600]'}`}>
                            {appt.status === 'approved' ? 'อนุมัติแล้ว' : 'รอครูตอบรับ'}
                         </span>
                       </div>
                       <p className="text-sm text-gray-600 mb-2">"{appt.reason}"</p>
                       
                       {/* Teacher Note Display */}
                       {appt.status === 'approved' && appt.teacherNote && (
                         <div className="bg-[#f0fff4] p-2 rounded-lg border border-[#c6f6d5] flex items-start gap-2">
                           <MessageCircle className="w-4 h-4 text-[#399d2f] mt-0.5 shrink-0" />
                           <div className="text-sm">
                             <span className="font-bold text-[#2f855a] block text-xs mb-0.5">ข้อความจากครู:</span>
                             <span className="text-[#276749]">{appt.teacherNote}</span>
                           </div>
                         </div>
                       )}
                    </div>
                  ))}
                </div>
             )}
           </div>

           {/* History List */}
           <div className="space-y-4">
             <h3 className="text-[#7193a6] text-lg font-semibold px-2">บันทึกที่ผ่านมา</h3>
             {entries.length === 0 && <p className="text-gray-400 text-center py-4">ยังไม่มีบันทึก</p>}
             {entries.map((entry) => (
               <div key={entry.id} className="bg-[#f3f1fc] rounded-[10px] p-[0.7em_1em] mb-[0.5em] border border-[#e5d9f7]">
                 <div className="flex justify-between items-start mb-2">
                   <span className="text-[0.93em] text-[#ae8cc1] font-bold">{entry.date}</span>
                   <span className="text-xs font-medium text-gray-500 bg-white px-2 py-1 rounded-lg border border-[#d3ecfd]">{entry.mood}</span>
                 </div>
                 <p className="text-gray-700 text-sm mb-3">{entry.text}</p>
                 {entry.aiResponse && (
                   <div className="bg-[#f1ffea] p-[1em_1.5em] rounded-[10px] mt-[0.9em] flex gap-3">
                     <div className="bg-white p-1.5 rounded-full h-fit shadow-sm border border-[#d3ecfd]">
                        <Sparkles className="w-3 h-3 text-[#a645ae]" />
                     </div>
                     <div>
                       <p className="text-xs font-bold text-[#a651b1] mb-1">พี่ AI แนะนำว่า:</p>
                       <p className="text-sm text-[#444] leading-relaxed">{entry.aiResponse}</p>
                     </div>
                   </div>
                 )}
               </div>
             ))}
           </div>
        </div>
      </div>
    </div>
  );
};

export default StudentDashboard;
import React, { useState } from 'react';
import { User, Appointment, MOCK_STUDENTS } from '../types';
import { Star, Calendar, CheckCircle, Clock, MessageCircle, PieChart as PieIcon, Plus, Minus, AlertCircle, Smile, Meh, Frown, Heart, CloudRain, Save } from 'lucide-react';
import { PieChart, Pie, Cell, ResponsiveContainer, Tooltip, Legend } from 'recharts';

interface TeacherDashboardProps {
  appointments: Appointment[];
  onApprove: (id: string, note: string) => void;
}

const TeacherDashboard: React.FC<TeacherDashboardProps> = ({ appointments, onApprove }) => {
  const [students, setStudents] = useState<User[]>(MOCK_STUDENTS);
  const [notes, setNotes] = useState<{ [key: string]: string }>({});
  const [confirmAction, setConfirmAction] = useState<{ studentId: string; amount: number; studentName: string } | null>(null);

  // Teacher Mood State
  const [teacherMood, setTeacherMood] = useState<string>('Happy');
  const [teacherNote, setTeacherNote] = useState('');
  const [myStats, setMyStats] = useState([
    { name: 'Happy', value: 8, color: '#b1e5e0' },
    { name: 'Neutral', value: 3, color: '#ffe780' },
    { name: 'Sad', value: 1, color: '#a651b1' },
    { name: 'Excited', value: 4, color: '#bfffa5' },
    { name: 'Anxious', value: 2, color: '#ffd7ef' },
  ]);

  const moods = [
    { label: 'Happy', icon: <Smile className="w-6 h-6" /> },
    { label: 'Neutral', icon: <Meh className="w-6 h-6" /> },
    { label: 'Sad', icon: <Frown className="w-6 h-6" /> },
    { label: 'Excited', icon: <Heart className="w-6 h-6" /> },
    { label: 'Anxious', icon: <CloudRain className="w-6 h-6" /> },
  ];

  const updateStars = (studentId: string, amount: number) => {
    setStudents(prev => prev.map(s => 
      s.id === studentId 
        ? { ...s, stars: Math.max(0, s.stars + amount) } 
        : s
    ));
  };

  const handleNoteChange = (id: string, value: string) => {
    setNotes(prev => ({ ...prev, [id]: value }));
  };

  const handleTeacherMoodSubmit = () => {
    if (!teacherNote.trim()) return;
    
    // Simulate updating stats
    setMyStats(prev => prev.map(stat => 
      stat.name === teacherMood 
        ? { ...stat, value: stat.value + 1 } 
        : stat
    ));
    
    setTeacherNote('');
    alert("บันทึกอารมณ์เรียบร้อยแล้วครับคุณครู! ✌️");
  };

  // Mock stats for the class
  const classStats = [
    { name: 'Happy', value: 12, color: '#b1e5e0' },
    { name: 'Neutral', value: 8, color: '#ffe780' },
    { name: 'Sad', value: 4, color: '#a651b1' },
    { name: 'Excited', value: 6, color: '#bfffa5' },
    { name: 'Anxious', value: 3, color: '#ffd7ef' },
  ];

  return (
    <div className="space-y-6 relative">
      <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] mb-[2em] shadow-[0_1px_18px_#e7e1fa60]">
        <h2 className="text-[#a645ae] text-2xl font-bold mt-0 letter-spacing-[.03em] mb-1">จัดการห้องเรียน 👩‍🏫</h2>
        <p className="text-[#7193a6]">ส่งกำลังใจให้เด็กๆ กันเถอะ</p>
      </div>

      {/* Teacher Self-Care Section */}
      <div className="grid md:grid-cols-2 gap-6">
        <div className="bg-[#fffefe] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60] border border-[#fcecfb]">
          <h3 className="text-[#a651b1] text-lg font-bold mb-4 flex items-center gap-2">
            <Heart className="w-5 h-5" /> Daily Check-in สำหรับครู
          </h3>
          <div className="space-y-4">
             <div className="flex flex-wrap gap-2">
              {moods.map((m) => (
                <button
                  key={m.label}
                  onClick={() => setTeacherMood(m.label)}
                  className={`flex-1 min-w-[50px] flex flex-col items-center justify-center p-2 rounded-xl border-[1.3px] transition-all duration-200 ${
                    teacherMood === m.label 
                      ? 'bg-[#ffd7ef] border-[#bb5ecf] scale-105 shadow-md' 
                      : 'bg-[#f7f9fd] border-[#e5e7eb] hover:bg-[#f0f9ff]'
                  }`}
                >
                  <div className={teacherMood === m.label ? 'text-[#a645ae]' : 'text-[#9ca3af]'}>
                      {m.icon}
                  </div>
                </button>
              ))}
            </div>
            <textarea
              value={teacherNote}
              onChange={(e) => setTeacherNote(e.target.value)}
              placeholder="วันนี้ครูเป็นอย่างไรบ้าง? (บันทึกส่วนตัว)"
              className="w-full p-3 text-sm border border-[#d3ecfd] rounded-lg bg-[#fbfdff] focus:border-[#a651b1] focus:outline-none resize-none h-20"
            />
            <button
              onClick={handleTeacherMoodSubmit}
              disabled={!teacherNote.trim()}
              className="w-full bg-[#b1e5e0] text-[#2c7a7b] font-bold py-2 rounded-lg hover:bg-[#9adfd9] transition-colors flex items-center justify-center gap-2 disabled:opacity-50 disabled:cursor-not-allowed"
            >
              <Save className="w-4 h-4" /> บันทึกความรู้สึก
            </button>
          </div>
        </div>

        <div className="bg-[#fffefe] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60] border border-[#fcecfb]">
          <h3 className="text-[#7193a6] text-lg font-bold mb-4 flex items-center gap-2">
            <PieIcon className="w-5 h-5 text-[#a651b1]" /> สถิติอารมณ์ของคุณครู
          </h3>
          <div className="h-40 w-full">
            <ResponsiveContainer width="100%" height="100%">
              <PieChart>
                <Pie
                  data={myStats}
                  cx="50%"
                  cy="50%"
                  innerRadius={35}
                  outerRadius={55}
                  paddingAngle={5}
                  dataKey="value"
                >
                  {myStats.map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={entry.color} stroke="none" />
                  ))}
                </Pie>
                <Tooltip 
                  contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
                />
                <Legend iconType="circle" wrapperStyle={{ fontSize: '11px' }} layout="vertical" align="right" verticalAlign="middle" />
              </PieChart>
            </ResponsiveContainer>
          </div>
        </div>
      </div>

      <div className="border-t border-[#e5d9f7] my-2"></div>

      <div className="grid md:grid-cols-2 gap-6">
        {/* Left Column: Student List */}
        <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
          <h3 className="text-[#7193a6] text-lg font-bold mb-4 flex items-center gap-2">
            <Star className="text-[#ffe780] fill-[#ffe780]" /> จัดการดาวเด็กดี
          </h3>
          <div className="space-y-3">
            {students.map(student => (
              <div key={student.id} className="flex items-center justify-between p-3 bg-white border border-[#d3ecfd] rounded-xl hover:bg-[#f3f1fc] transition-colors">
                <div className="flex items-center gap-3">
                  <div className="w-10 h-10 bg-[#e9dfff] rounded-full flex items-center justify-center text-[#86398e] font-bold">
                    {student.name.charAt(0)}
                  </div>
                  <div>
                    <p className="font-semibold text-[#444]">{student.name}</p>
                    <p className="text-xs text-[#7193a6] font-medium">{student.stars} ดวง</p>
                  </div>
                </div>
                <div className="flex items-center gap-2">
                  <button 
                    onClick={() => setConfirmAction({ studentId: student.id, amount: -1, studentName: student.name })}
                    className="w-8 h-8 flex items-center justify-center bg-[#ffd7ef] hover:bg-[#ffaad4] text-[#d13d84] rounded-lg transition-all active:scale-90"
                    title="ลดดาว"
                    aria-label={`Decrease stars for ${student.name}`}
                  >
                    <Minus className="w-4 h-4" />
                  </button>
                  <button 
                    onClick={() => setConfirmAction({ studentId: student.id, amount: 1, studentName: student.name })}
                    className="w-8 h-8 flex items-center justify-center bg-[#ffe780] hover:bg-[#ffdf4d] text-[#764108] rounded-lg transition-all active:scale-90"
                    title="เพิ่มดาว"
                    aria-label={`Increase stars for ${student.name}`}
                  >
                    <Plus className="w-4 h-4" />
                  </button>
                </div>
              </div>
            ))}
          </div>
        </div>

        {/* Right Column: Chart & Appointments */}
        <div className="space-y-6">
          {/* Class Emotion Chart */}
          <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
             <h3 className="text-[#7193a6] text-lg font-bold mb-4 flex items-center gap-2">
               <PieIcon className="text-[#a651b1]" /> ภาพรวมอารมณ์ห้องเรียน
             </h3>
             <div className="h-48 w-full">
               <ResponsiveContainer width="100%" height="100%">
                 <PieChart>
                   <Pie
                     data={classStats}
                     cx="50%"
                     cy="50%"
                     innerRadius={40}
                     outerRadius={60}
                     paddingAngle={5}
                     dataKey="value"
                   >
                     {classStats.map((entry, index) => (
                       <Cell key={`cell-${index}`} fill={entry.color} stroke="none" />
                     ))}
                   </Pie>
                   <Tooltip 
                     contentStyle={{ borderRadius: '12px', border: 'none', boxShadow: '0 4px 12px rgba(0,0,0,0.1)' }}
                   />
                   <Legend iconType="circle" wrapperStyle={{ fontSize: '12px' }} />
                 </PieChart>
               </ResponsiveContainer>
             </div>
           </div>

           {/* Appointments */}
           <div className="bg-[#f7f9fd] rounded-[15px] p-[1.35em_2em] shadow-[0_1px_18px_#e7e1fa60]">
              <h3 className="text-[#7193a6] text-lg font-bold mb-4 flex items-center gap-2">
               <Calendar className="text-[#a651b1]" /> นัดหมายปรึกษา
             </h3>
             <div className="space-y-3">
               {appointments.length === 0 && <p className="text-gray-400 text-center py-4">ไม่มีนัดหมายใหม่</p>}
               {appointments.map(appt => (
                 <div key={appt.id} className="p-4 border border-[#d3ecfd] bg-white rounded-xl">
                    <div className="flex justify-between items-start mb-2">
                       <span className="font-bold text-[#a645ae]">{appt.studentName}</span>
                       <span className={`text-xs px-2 py-1 rounded-full ${appt.status === 'approved' ? 'bg-[#d9fff5] text-[#399d2f]' : 'bg-[#fff5d6] text-[#b38600]'}`}>
                         {appt.status === 'approved' ? 'อนุมัติแล้ว' : 'รออนุมัติ'}
                       </span>
                    </div>
                    <div className="text-sm text-[#7193a6] mb-3 space-y-1">
                       <div className="flex items-center gap-2"><Clock className="w-4 h-4"/> {appt.date}</div>
                       <div className="opacity-80">"{appt.reason}"</div>
                    </div>
                    
                    {appt.status === 'pending' ? (
                      <div className="mt-3">
                        <div className="flex items-center gap-2 mb-2 text-[#7193a6] text-sm">
                          <MessageCircle className="w-4 h-4" />
                          <span>ข้อความถึงนักเรียน:</span>
                        </div>
                        <textarea
                           value={notes[appt.id] || ''}
                           onChange={(e) => handleNoteChange(appt.id, e.target.value)}
                           placeholder="เช่น: ยินดีครับ เดี๋ยวครูรอที่ห้องพักครูนะ..."
                           className="w-full p-2 mb-3 text-sm border border-[#d3ecfd] rounded-lg bg-[#fff6f8] focus:border-[#a651b1] focus:outline-none resize-none"
                           rows={2}
                        />
                        <button 
                         onClick={() => onApprove(appt.id, notes[appt.id] || '')}
                         className="w-full py-2 bg-[#ffc285] text-[#764108] font-bold rounded-lg hover:bg-[#ffb366] flex justify-center items-center gap-2 shadow-[0_1px_5px_#ede0fb40]"
                        >
                          <CheckCircle className="w-4 h-4" /> ยืนยันนัดหมาย
                        </button>
                      </div>
                    ) : (
                      appt.teacherNote && (
                        <div className="mt-3 bg-[#f0fff4] border border-[#c6f6d5] p-3 rounded-lg flex items-start gap-3">
                           <div className="bg-[#c6f6d5] p-1.5 rounded-full text-[#2f855a] shrink-0">
                             <MessageCircle className="w-4 h-4" />
                           </div>
                           <div>
                              <p className="text-xs font-bold text-[#2f855a] mb-0.5">ข้อความถึงนักเรียน:</p>
                              <p className="text-sm text-[#276749]">{appt.teacherNote}</p>
                           </div>
                        </div>
                      )
                    )}
                 </div>
               ))}
             </div>
           </div>
        </div>
      </div>

      {/* Confirmation Modal */}
      {confirmAction && (
        <div className="fixed inset-0 bg-black/50 flex items-center justify-center z-50 animate-fade-in backdrop-blur-sm">
          <div className="bg-white rounded-2xl p-6 w-[90%] max-w-sm shadow-2xl transform transition-all scale-100 border-2 border-white">
            <h3 className="text-xl font-bold text-[#a645ae] mb-3 flex items-center gap-2">
              {confirmAction.amount > 0 ? (
                <Star className="w-6 h-6 fill-[#ffe780] text-[#ffe780]" />
              ) : (
                <AlertCircle className="w-6 h-6 text-[#d13d84]" />
              )}
              {confirmAction.amount > 0 ? 'มอบดาวเด็กดี' : 'ลดดาว'}
            </h3>
            <p className="text-gray-600 mb-6 leading-relaxed">
              ยืนยันการ{confirmAction.amount > 0 ? 'เพิ่ม' : 'ลด'} 1 ดาว ให้กับ <br/>
              <span className="font-bold text-[#444] text-lg">{confirmAction.studentName}</span> ใช่หรือไม่?
            </p>
            <div className="flex gap-3 justify-end">
              <button 
                onClick={() => setConfirmAction(null)}
                className="px-4 py-2 rounded-xl text-gray-500 hover:bg-gray-100 font-medium transition-colors"
              >
                ยกเลิก
              </button>
              <button 
                onClick={() => {
                  updateStars(confirmAction.studentId, confirmAction.amount);
                  setConfirmAction(null);
                }}
                className={`px-5 py-2 rounded-xl font-bold shadow-md transition-all transform active:scale-95 ${
                  confirmAction.amount > 0 
                    ? 'bg-[#ffe780] text-[#764108] hover:bg-[#ffdf4d]' 
                    : 'bg-[#ffd7ef] text-[#d13d84] hover:bg-[#ffaad4]'
                }`}
              >
                ยืนยัน
              </button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
};

export default TeacherDashboard;
import { GoogleGenAI } from "@google/genai";

// Initialize the API client
// Note: In a real environment, ensure process.env.API_KEY is set.
const ai = new GoogleGenAI({ apiKey: process.env.API_KEY });

export const getSupportiveAdvice = async (
  diaryText: string, 
  mood: string
): Promise<string> => {
  try {
    const model = 'gemini-2.5-flash';
    
    const prompt = `
      You are a kind, empathetic, and supportive school counselor AI for elementary/middle school students in Thailand.
      
      Context:
      The student is feeling: ${mood}
      The student wrote in their diary: "${diaryText}"

      Task:
      Provide a very short, warm, and encouraging response (max 2 sentences) in the Thai language. 
      Validate their feelings and offer a tiny bit of hope or advice.
      Do not be overly clinical; be like a supportive older sibling or favorite teacher.
    `;

    const response = await ai.models.generateContent({
      model: model,
      contents: prompt,
    });

    return response.text || "ขอโทษนะจ๊ะ ครู AI ไม่สามารถตอบกลับได้ในขณะนี้ (Error generating text)";
  } catch (error) {
    console.error("Gemini API Error:", error);
    return "ระบบ AI กำลังพักผ่อน พยายามใหม่อีกครั้งนะจ๊ะ";
  }
};
</html>
