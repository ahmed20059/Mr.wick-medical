# Mr.wick-medical
import { useState } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { Input } from "@/components/ui/input"; import { Textarea } from "@/components/ui/textarea";

const initialData = { "تشريح": { videos: [ { title: "القلب", url: "https://www.youtube.com/embed/dummy1" }, { title: "الرئتين", url: "https://www.youtube.com/embed/dummy2" } ], exams: [ { question: "كم عدد حجرات القلب؟", options: ["2", "3", "4"], answer: "4" } ] }, "فسلجة": { videos: [ { title: "الكلى", url: "https://www.youtube.com/embed/dummy3" } ], exams: [ { question: "ما وظيفة الكلى؟", options: ["الهضم", "تنقية الدم", "التنفس"], answer: "تنقية الدم" } ] } };

export default function HomePage() { const [loggedIn, setLoggedIn] = useState(false); const [isAdmin, setIsAdmin] = useState(false); const [isRegistering, setIsRegistering] = useState(false); const [username, setUsername] = useState(""); const [password, setPassword] = useState(""); const [studentInfo, setStudentInfo] = useState({ name: "", age: "", stage: "", university: "", email: "" }); const [selectedMaterial, setSelectedMaterial] = useState(null); const [section, setSection] = useState("videos"); const [data, setData] = useState(initialData); const [newVideo, setNewVideo] = useState({ title: "", url: "" });

const handleLogin = () => { if (username === "admin" && password === "admin123") { setIsAdmin(true); setLoggedIn(true); } else if (username && password) { setLoggedIn(true); } else { alert("بيانات الدخول غير صحيحة"); } };

const handleRegister = () => { if ( studentInfo.name && studentInfo.age && studentInfo.stage && studentInfo.university && studentInfo.email ) { alert("تم إنشاء الحساب بنجاح! يمكنك الآن تسجيل الدخول."); setIsRegistering(false); } else { alert("يرجى ملء جميع الحقول"); } };

const handleAddVideo = () => { if (!newVideo.title || !newVideo.url || !selectedMaterial) return; const updated = { ...data }; updated[selectedMaterial].videos.push({ ...newVideo }); setData(updated); setNewVideo({ title: "", url: "" }); };

if (!loggedIn) { return ( <div className="min-h-screen flex flex-col items-center justify-center bg-gradient-to-br from-blue-100 to-white relative"> <img
src="https://img.freepik.com/premium-photo/doctor-interface-hologram-medical-technology-futuristic-hospital-digital-display_41969-10491.jpg"
className="absolute top-0 left-0 w-full h-full object-cover opacity-10"
alt="background"
/> <div className="absolute top-4 right-4 text-sm font-bold text-gray-700">Mr.Wick</div>

{isRegistering ? (
      <div className="z-10 p-6 bg-white/90 rounded-2xl shadow-xl max-w-sm w-full">
        <h1 className="text-xl font-bold mb-4 text-center">إنشاء حساب طالب</h1>
        <Input className="mb-2" placeholder="الاسم الثلاثي" value={studentInfo.name} onChange={(e) => setStudentInfo({ ...studentInfo, name: e.target.value })} />
        <Input className="mb-2" placeholder="العمر" value={studentInfo.age} onChange={(e) => setStudentInfo({ ...studentInfo, age: e.target.value })} />
        <Input className="mb-2" placeholder="المرحلة الدراسية" value={studentInfo.stage} onChange={(e) => setStudentInfo({ ...studentInfo, stage: e.target.value })} />
        <Input className="mb-2" placeholder="اسم الجامعة" value={studentInfo.university} onChange={(e) => setStudentInfo({ ...studentInfo, university: e.target.value })} />
        <Input className="mb-4" placeholder="البريد الإلكتروني" value={studentInfo.email} onChange={(e) => setStudentInfo({ ...studentInfo, email: e.target.value })} />
        <Button className="w-full mb-2" onClick={handleRegister}>تسجيل</Button>
        <Button variant="outline" className="w-full" onClick={() => setIsRegistering(false)}>العودة لتسجيل الدخول</Button>
      </div>
    ) : (
      <div className="z-10 p-6 bg-white/90 rounded-2xl shadow-xl max-w-sm w-full">
        <h1 className="text-xl font-bold mb-4 text-center">تسجيل الدخول</h1>
        <Input
          placeholder="اسم المستخدم"
          className="mb-3"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
        <Input
          placeholder="كلمة المرور"
          type="password"
          className="mb-4"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <Button className="w-full mb-2" onClick={handleLogin}>
          دخول
        </Button>
        <Button variant="outline" className="w-full" onClick={() => setIsRegistering(true)}>
          إنشاء حساب جديد
        </Button>
      </div>
    )}
  </div>
);

}

if (!selectedMaterial) { return ( <div className="p-4 min-h-screen bg-blue-50"> <h1 className="text-2xl font-bold mb-4 text-center">اختر المادة</h1> <div className="grid gap-4"> {Object.keys(data).map((material) => ( <Button key={material} onClick={() => setSelectedMaterial(material)}> {material} </Button> ))} </div> </div> ); }

const materialData = data[selectedMaterial];

return ( <div className="p-4 min-h-screen bg-blue-50"> <div className="flex justify-between items-center mb-4"> <Button variant="outline" onClick={() => setSelectedMaterial(null)}> ← رجوع </Button> <h2 className="text-xl font-bold">{selectedMaterial}</h2> </div>

<div className="flex gap-4 mb-4">
    <Button onClick={() => setSection("videos")} variant={section === "videos" ? "default" : "outline"}>الفيديوهات</Button>
    <Button onClick={() => setSection("exams")} variant={section === "exams" ? "default" : "outline"}>الامتحانات</Button>
    {isAdmin && (
      <Button onClick={() => setSection("upload")} variant={section === "upload" ? "default" : "outline"}>رفع فيديو</Button>
    )}
  </div>

  {section === "videos" && (
    <div className="grid gap-6">
      {materialData.videos.map((video, index) => (
        <Card key={index}>
          <CardContent>
            <h3 className="font-bold mb-2">{video.title}</h3>
            <iframe
              width="100%"
              height="200"
              src={video.url}
              title={video.title}
              frameBorder="0"
              allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowFullScreen
            ></iframe>
          </CardContent>
        </Card>
      ))}
    </div>
  )}

  {section === "exams" && (
    <div className="grid gap-6">
      {materialData.exams.map((exam, index) => (
        <Card key={index}>
          <CardContent>
            <h3 className="font-bold mb-2">{exam.question}</h3>
            {exam.options.map((opt, i) => (
              <div key={i} className="mb-1">
                <label className="flex items-center gap-2">
                  <input type="radio" name={`q-${index}`} value={opt} />
                  {opt}
                </label>
              </div>
            ))}
          </CardContent>
        </Card>
      ))}
    </div>
  )}

  {section === "upload" && isAdmin && (
    <Card className="mt-6">
      <CardContent>
        <h3 className="font-bold mb-4">رفع فيديو جديد</h3>
        <Input
          placeholder="عنوان الفيديو"
          className="mb-2"
          value={newVideo.title}
          onChange={(e) => setNewVideo({ ...newVideo, title: e.target.value })}
        />
        <Input
          placeholder="رابط الفيديو (YouTube embed)"
          className="mb-4"
          value={newVideo.url}
          onChange={(e) => setNewVideo({ ...newVideo, url: e.target.value })}
        />
        <Button onClick={handleAddVideo}>إضافة</Button>
      </CardContent>
    </Card>
  )}
</div>

); }

