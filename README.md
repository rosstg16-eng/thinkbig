import React, { useEffect, useMemo, useState } from "react";
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Checkbox } from "@/components/ui/checkbox";
import { Textarea } from "@/components/ui/textarea";
import { Progress } from "@/components/ui/progress";
import { Switch } from "@/components/ui/switch";
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import { Select, SelectContent, SelectItem, SelectTrigger, SelectValue } from "@/components/ui/select";
import { Bug, PartyPopper, Sparkles } from "lucide-react";

// -------------------------------------------------
// 🎨 Brand (Fika-inspired)
// -------------------------------------------------
const brand = {
  cocoa: "#3a160f", // deep cocoa bg
  shadow: "#2a0e08", // darker inner shadow
  coral: "#ff6236", // warm coral/orange
  blush: "#ff7bbd", // pink
  lilac: "#c59bff", // lilac
  peach: "#ffb38a", // peach
  olive: "#779a4a", // olive frame
  lime: "#c2e86b", // lime accent
  cream: "#fff7ec", // soft cream
  sand: "#ffe6cf", // sand card
};
const gradients = [
  `linear-gradient(135deg, ${brand.lilac}, ${brand.blush})`,
  `linear-gradient(135deg, ${brand.peach}, ${brand.coral})`,
  `linear-gradient(135deg, ${brand.blush}, ${brand.peach})`,
];

function useBrandFonts() {
  useEffect(() => {
    const id = "brand-fonts-link";
    if (document.getElementById(id)) return;
    const link = document.createElement("link");
    link.id = id;
    link.rel = "stylesheet";
    link.href =
      "https://fonts.googleapis.com/css2?family=Fraunces:wght@700;900&family=Nunito+Sans:wght@400;600;700;800&display=swap";
    document.head.appendChild(link);
  }, []);
}

function PhoneShell({ children }) {
  useBrandFonts();
  return (
    <div className="min-h-screen" style={{ backgroundColor: brand.cocoa }}>
      <div
        className="max-w-md mx-auto my-4 rounded-[36px] overflow-hidden shadow-xl"
        style={{
          background: `radial-gradient(120% 80% at 100% 0%, ${brand.cream} 0%, #fff 40%, ${brand.peach}20 100%)`,
          fontFamily: "'Nunito Sans', system-ui, -apple-system, Segoe UI, Roboto, sans-serif",
          boxShadow: "0 10px 40px rgba(0,0,0,.35)",
        }}
      >
        <style
          dangerouslySetInnerHTML={{
            __html: `
              .brand-title{font-family:'Fraunces',serif;font-weight:800}
              .btn-brand{border-radius:9999px;height:2.25rem;padding:0 .9rem;font-size:11px;font-weight:800;letter-spacing:.04em;text-transform:uppercase;background:${brand.coral}!important;color:#fff!important;border:1px solid ${brand.coral}!important;box-shadow:0 6px 0 ${brand.shadow}22}
              .btn-outline-brand{border-radius:9999px;height:2.25rem;padding:0 .9rem;border:1px solid ${brand.peach};}
            `,
          }}
        />
        {children}
      </div>
    </div>
  );
}

function BrandHero() {
  return (
    <div className="relative">
      <div className="h-28" style={{ background: gradients[0] }} />
      <div className="px-4 pt-4 pb-3">
        <div className="text-[11px] tracking-wider font-extrabold uppercase" style={{ color: brand.coral }}>
          HELLO GORGEOUS!
        </div>
        <h1 className="text-2xl brand-title leading-tight tracking-tight" style={{ color: brand.cocoa }}>
          Taylor’s Focus Sprint
        </h1>
        <p className="text-xs" style={{ color: "#6b7280" }}>
          Now → Nov 21 • Soft discipline, slow habits
        </p>
      </div>
    </div>
  );
}

// Scalloped card that mirrors the left reference panel (olive frame + cream scallops)
function ScallopCard({ title, children }) {
  return (
    <div className="rounded-[28px] overflow-hidden" style={{ background: brand.olive, boxShadow: "0 10px 20px rgba(0,0,0,.15)" }}>
      <div
        className="p-4"
        style={{
          background:
            `radial-gradient(circle at 14px 14px, ${brand.cream} 12px, transparent 13px) 0 0/28px 28px, ` +
            `${brand.cream}`,
        }}
      >
        <div className="max-w-none rounded-[20px] p-3" style={{ background: brand.sand, boxShadow: `0 6px 0 ${brand.shadow}22` }}>
          <div className="text-sm font-extrabold mb-2" style={{ color: brand.cocoa }}>{title}</div>
          {children}
        </div>
      </div>
    </div>
  );
}

// -------------------------------------------------
// 📅 Data / Generators (concise)
// -------------------------------------------------
const START_DATE = new Date("2025-09-26T00:00:00");
const HINGE_REVIEW_DATE = new Date("2025-11-22T00:00:00");
const WEEKS = 8;
const DAY_MS = 86400000;
const SHOW_TESTS_DEFAULT = true;

const SCRIPTURES = ["Psalm 23","Proverbs 3:5–6","Isaiah 41:10","Philippians 4:6–7","Romans 12:2","Matthew 5:16","Psalm 27"];
const BUSINESS_TOPICS = ["Hand therapy niches: racquet athletes","Hand therapy niches: climbers","Creator/desk athlete ergonomics","Post-op nerve & scar protocols","Musicians & stylists: grip & endurance","Gamers: thumb/wrist load management","Local market scan + pricing comps"];
const FOOD_TASKS = ["Hit 130–150 g protein (log it)","2 servings of veggies (greens + color)","Cook all meals at home today","No fast food / DoorDash","Prep tomorrow’s protein (season/cook)","Build a salad kit / fruit box","Plan 3 meals for tomorrow"];

const formatDate = (d) => d.toLocaleDateString(undefined, { month: "short", day: "numeric" });
function useLocalStorage(key, initialValue) {
  const [value, setValue] = useState(() => {
    try { const v = typeof window !== "undefined" ? localStorage.getItem(key) : null; return v ? JSON.parse(v) : initialValue; } catch { return initialValue; }
  });
  useEffect(() => { try { if (typeof window !== "undefined") localStorage.setItem(key, JSON.stringify(value)); } catch {} }, [key, value]);
  return [value, setValue];
}

function fitnessTasks(d){return [["Booty day (glutes/hamstrings 45–60m)","8–10k steps or 20m walk"],["Back day (rows/pull 45–60m)","15–20m easy cardio"],["Cardio intervals (20–30m)","Core 10m"],["Glute activation + hamstrings (45m)","10–15m mobility"],["Upper back/posture (45m)","10–15m incline walk"],["Long walk/hike (45–60m)","Stretch 10m"],["Yoga/mobility (20–30m)","Leisure walk 20m"]][d];}
function capstoneTasks(d){return [["IRB: Problem & Aims (draft/update)","Create IRB ‘Revisions’ doc"],["IRB: Methods & Procedures (session flow, measures)","Data security paragraph"],["IRB: Risks/Benefits + Consent edits","Compile to PDF checklist"],["Poster: Title/Abstract 3-sentence version","Select 1 figure/table frame"],["Poster: Methods graphic (boxes/arrows)","Results placeholders (n, measures)"],["Capstone: lit scan 2 new sources","Write 3 discussion bullets"],["Mentor email: 3 questions + attach draft","Reference tidy (APA)"]][d];}
function businessTasks(d){return [BUSINESS_TOPICS[d], d===1?"Set Google Alerts: ‘carpal tunnel outcomes’, ‘tennis elbow loading’, ‘hand therapy Virginia’": d===4?"Outline 3 content ideas for this niche": d===6?"Draft 1-page service offer (who/what/price/outcome)":"Save 2 useful links + notes" ];}
function homeTasks(d){return [["Kitchen reset + dishes","Take trash out"],["Laundry: darks","Fold & put away 10m"],["Bathroom surfaces 10m","Vacuum high-traffic 10m"],["Laundry: lights","Clear hotspots (desk/nightstand)"],["Fridge tidy + grocery mini-list","Mop/spot clean 10m"],["Deep tidy 30m or donate bag","Wipe mirrors & handles"],["Change sheets & towels","Plan outfits/gear for week"]][d];}
function generateTasksForDay(i){const list=[];(fitnessTasks(i)||[]).forEach((t,k)=>list.push({id:`fit-${i}-${k}`,cat:"Fitness",label:t,done:false}));[FOOD_TASKS[i],FOOD_TASKS[(i+1)%FOOD_TASKS.length],FOOD_TASKS[(i+2)%FOOD_TASKS.length]].forEach((t,k)=>list.push({id:`food-${i}-${k}`,cat:"Food",label:t,done:false}));(capstoneTasks(i)||[]).forEach((t,k)=>list.push({id:`cap-${i}-${k}`,cat:"Capstone/IRB/Poster",label:t,done:false}));list.push({id:`faith-${i}-0`,cat:"Faith",label:`Read ${SCRIPTURES[i]}`,done:false});list.push({id:`faith-${i}-1`,cat:"Faith",label:"10–20m devotional / prayer",done:false});businessTasks(i).forEach((t,k)=>list.push({id:`biz-${i}-${k}`,cat:"Business",label:t,done:false}));(homeTasks(i)||[]).forEach((t,k)=>list.push({id:`home-${i}-${k}`,cat:"Home",label:t,done:false}));return list;}

const WEEKS_ARR = Array.from({ length: 8 }, (_, i) => ({
  week: i + 1,
  goals: ["IRB/Capstone progress on schedule","50 boards questions","4 workouts","Home resets 5/7","Invisalign 14/14","2 biz/social planning blocks"],
}));
function buildDefaultWeeks(){const out=[];for(let i=0;i<8;i++){const start=new Date(START_DATE.getTime()+i*7*DAY_MS),end=new Date(start.getTime()+6*DAY_MS);out.push({index:i,title:`Week ${i+1} (${formatDate(start)}–${formatDate(end)})`,range:[start.toISOString(),end.toISOString()],goals:(WEEKS_ARR[i]?.goals||[]).map((g,gi)=>({id:`g-${i}-${gi}`,label:g,done:false})),questions:0,workouts:0,bizBlocks:0,notes:"",days:Array.from({length:7},(_,d)=>({date:new Date(start.getTime()+d*DAY_MS).toISOString(),tasks:generateTasksForDay(d)}))});}return out;}
function needsMigration(weeks){try{if(!Array.isArray(weeks)||!weeks.length)return false;const w0=weeks[0];if(!w0?.days||!Array.isArray(w0.days))return true;return w0.days.some(d=>!Array.isArray(d.tasks));}catch{return true;}}
function migrateWeeks(weeks){if(!Array.isArray(weeks))return buildDefaultWeeks();return weeks.map((w,wi)=>{const start=new Date(START_DATE.getTime()+wi*7*DAY_MS);const days=(w.days||[]).map((d,di)=>({date:d?.date?d.date:new Date(start.getTime()+di*DAY_MS).toISOString(),tasks:Array.isArray(d?.tasks)?d.tasks:generateTasksForDay(di)}));const finalDays=days.length===7?days:Array.from({length:7},(_,di)=>({date:new Date(start.getTime()+di*DAY_MS).toISOString(),tasks:generateTasksForDay(di)}));return {index:typeof w.index==="number"?w.index:wi,title:w.title||`Week ${wi+1}`,range:Array.isArray(w.range)&&w.range.length===2?w.range:[new Date(start).toISOString(),new Date(start.getTime()+6*DAY_MS).toISOString()],goals:Array.isArray(w.goals)?w.goals:(WEEKS_ARR[wi]?.goals||[]).map((g,gi)=>({id:`g-${wi}-${gi}`,label:g,done:false})),questions:typeof w.questions==="number"?w.questions:0,workouts:typeof w.workouts==="number"?w.workouts:0,bizBlocks:typeof w.bizBlocks==="number"?w.bizBlocks:0,notes:typeof w.notes==="string"?w.notes:"",days:finalDays};});}

// Tests (kept)
function runSmokeTests(){const R=[];try{const weeks=buildDefaultWeeks();R.push({name:"weeks len",pass:weeks.length===8});R.push({name:"week has 7 days",pass:weeks[0].days.length===7});const d0=weeks[0].days[0].tasks.map(t=>t.label).join("|");const d1=weeks[0].days[1].tasks.map(t=>t.label).join("|");R.push({name:"unique days",pass:d0!==d1});const pct=Math.max(0,Math.min(100,((210-210)/(210-170))*100||0));R.push({name:"weight pct start",pass:pct===0});const bad=["Progress &gt; perfection."].some(s=>s.includes(">"));R.push({name:"no raw >",pass:!bad});const mig=migrateWeeks([{index:0,title:"W1",range:[new Date("2025-09-26").toISOString(),new Date("2025-10-02").toISOString()],goals:[],days:Array.from({length:7},()=>({date:new Date("2025-09-26").toISOString(),checks:{}}))}]);R.push({name:"migration has tasks",pass:Array.isArray(mig[0].days[0].tasks)&&mig[0].days[0].tasks.length>0});const cats=new Set(generateTasksForDay(0).map(t=>t.cat));const need=["Fitness","Food","Capstone/IRB/Poster","Faith","Business","Home"];R.push({name:"all cats present",pass:need.every(c=>cats.has(c))});}catch(e){R.push({name:"tests crashed",pass:false,detail:String(e)});}return R;}

function CounterCard({ title, gradient, value, onDec, onInc, note }){
  return (
    <div className="p-2 rounded-xl text-xs" style={{ background: gradient }}>
      <div className="text-slate-800">{title}</div>
      <div className="flex items-center gap-2 mt-1">
        <Button className="btn-outline-brand" variant="outline" onClick={onDec}>−</Button>
        <div className="text-lg font-bold w-10 text-center">{value}</div>
        <Button className="btn-outline-brand" variant="outline" onClick={onInc}>+</Button>
      </div>
      {note && <div className="mt-1" style={{ color: "#475569" }}>{note}</div>}
    </div>
  );
}

// -------------------------------------------------
// 🧩 App
// -------------------------------------------------
export default function App(){
  const [weeks,setWeeks]=useLocalStorage("taylor-focus-weeks",buildDefaultWeeks());
  const [active,setActive]=useLocalStorage("taylor-focus-active-week",0);
  const [niche,setNiche]=useLocalStorage("taylor-niche","I help racquet athletes and climbers return to pain-free play without setbacks using graded loading, manual therapy, and smart splinting.");
  const [weight,setWeight]=useLocalStorage("taylor-weight-track",{start:210,current:210,goal:170});
  const [showTests,setShowTests]=useLocalStorage("taylor-show-tests",true);

  useEffect(()=>{ if(needsMigration(weeks)) setWeeks(prev=>migrateWeeks(prev)); /* eslint-disable-next-line */},[]);
  const week=weeks[active]||weeks[0];
  const updateWeek=(fn)=>setWeeks(prev=>{const copy=[...prev]; const next=fn({...copy[active]}); copy[active]=next; return copy;});

  const todayIdx=useMemo(()=>{ const idx=week?.days?.findIndex?.(d=>new Date(d.date).toDateString()===new Date().toDateString()); return typeof idx==="number"&&idx>=0?idx:0;},[week]);
  const [selectedDay,setSelectedDay]=useLocalStorage("taylor-selected-day",todayIdx);
  useEffect(()=>{ setSelectedDay(p=> (p>=0&&p<7?p:todayIdx)); /* eslint-disable-next-line */},[active]);
  const selected=(week?.days&&week.days[selectedDay])||(week?.days&&week.days[0])||{date:new Date().toISOString(),tasks:[]};

  const allChecksComplete=useMemo(()=>selected.tasks?.every(t=>t.done)??false,[selected]);
  const weekProgress=useMemo(()=>{ if(!week?.days) return 0; const gc=week.goals?.length||0; const gd=(week.goals||[]).filter(g=>g.done).length; const tt=week.days.reduce((a,d)=>a+((d.tasks&&d.tasks.length)||0),0); const dt=week.days.reduce((a,d)=>a+((d.tasks&&d.tasks.filter(t=>t.done).length)||0),0); const q=Math.min(week.questions||0,50)/50, w=Math.min(week.workouts||0,4)/4, b=Math.min(week.bizBlocks||0,2)/2; const parts=[gc?gd/gc:0, tt?dt/tt:0, q, w, b]; return Math.round((parts.reduce((a,c)=>a+c,0)/parts.length)*100); },[week]);
  const weekLabel=`Week ${week.index+1} (${formatDate(new Date(week.range[0]))}–${formatDate(new Date(week.range[1]))})`;
  const weightPct=useMemo(()=>{ const pct=((weight.start-weight.current)/(weight.start-weight.goal))*100; return Math.max(0,Math.min(100,pct||0)); },[weight]);

  return (
    <PhoneShell>
      <BrandHero />

      <div className="px-4 pb-4 space-y-4">
        {/* Overview */}
        <Card className="shadow-sm" style={{ borderColor: `${brand.peach}60` }}>
          <CardHeader className="pb-2"><CardTitle className="text-base brand-title" style={{ color: brand.cocoa }}>Sprint Overview</CardTitle></CardHeader>
          <CardContent className="space-y-3">
            <div className="flex items-center justify-between">
              <div className="text-sm" style={{ color: "#475569" }}>{weekLabel}</div>
              <Select value={String(active)} onValueChange={(v)=>setActive(Number(v))}>
                <SelectTrigger className="w-28 h-8 text-xs"><SelectValue /></SelectTrigger>
                <SelectContent>{weeks.map((w,i)=>(<SelectItem key={i} value={String(i)}>Week {i+1}</SelectItem>))}</SelectContent>
              </Select>
            </div>
            <div className="space-y-1">
              <div className="flex items-center justify-between text-sm"><span>Overall progress</span><span className="font-medium">{weekProgress}%</span></div>
              <Progress value={weekProgress} />
            </div>
            {new Date()>=HINGE_REVIEW_DATE && (<div className="text-[11px]" style={{ color: "#6b7280" }}>Gentle nudge: dating app check‑in is due.</div>)}
            <div className="grid grid-cols-3 gap-2">
              <CounterCard title="Boards" gradient={gradients[2]} value={week.questions||0} onDec={()=>updateWeek(w=>({...w,questions:Math.max(0,(w.questions||0)-5)}))} onInc={()=>updateWeek(w=>({...w,questions:(w.questions||0)+5}))} note="Goal 50/wk" />
              <CounterCard title="Workouts" gradient={gradients[1]} value={week.workouts||0} onDec={()=>updateWeek(w=>({...w,workouts:Math.max(0,(w.workouts||0)-1)}))} onInc={()=>updateWeek(w=>({...w,workouts:(w.workouts||0)+1}))} note="Goal 4" />
              <CounterCard title="Biz/Social" gradient={gradients[0]} value={week.bizBlocks||0} onDec={()=>updateWeek(w=>({...w,bizBlocks:Math.max(0,(w.bizBlocks||0)-1)}))} onInc={()=>updateWeek(w=>({...w,bizBlocks:(w.bizBlocks||0)+1}))} note="Goal 2" />
            </div>
          </CardContent>
        </Card>

        {/* Tabs */}
        <Tabs defaultValue="daily" className="w-full">
          <TabsList className="grid grid-cols-3 rounded-xl" style={{ background: brand.cream }}>
            <TabsTrigger value="daily">Daily</TabsTrigger>
            <TabsTrigger value="weekly">Weekly</TabsTrigger>
            <TabsTrigger value="notes">Notes</TabsTrigger>
          </TabsList>

          {/* Daily */}
          <TabsContent value="daily" className="mt-2">
            <ScallopCard title="Daily Plan">
              <div className="flex items-center gap-2 mb-2">
                <Button className="btn-outline-brand" variant="outline" onClick={()=>setSelectedDay(d=>Math.max(0,d-1))}>Prev</Button>
                <Select value={String(selectedDay)} onValueChange={(v)=>setSelectedDay(Number(v))}>
                  <SelectTrigger className="h-9"><SelectValue placeholder="Pick a day" /></SelectTrigger>
                  <SelectContent>{week.days.map((d,i)=>(<SelectItem key={i} value={String(i)}>{formatDate(new Date(d.date))}</SelectItem>))}</SelectContent>
                </Select>
                <Button className="btn-outline-brand" variant="outline" onClick={()=>setSelectedDay(d=>Math.min(6,d+1))}>Next</Button>
              </div>

              {allChecksComplete && (
                <div className="p-3 rounded-xl mb-2" style={{ background: `${brand.lime}40`, color: brand.cocoa }}>
                  <span className="inline-flex items-center gap-2"><PartyPopper className="w-4 h-4"/> Win locked in! <Sparkles className="w-4 h-4"/></span>
                </div>
              )}

              <div className="rounded-2xl border p-3" style={{ borderColor: `${brand.peach}60` }}>
                <div className="text-xs mb-2" style={{ color: "#6b7280" }}>{formatDate(new Date(selected.date))}</div>
                {(() => {
                  const grouped=(selected.tasks||[]).reduce((a,t)=>{(a[t.cat]=a[t.cat]||[]).push(t);return a;},{});
                  return Object.entries(grouped).map(([cat,items])=> (
                    <div key={cat} className="mb-3">
                      <div className="text-[12px] font-semibold mb-1" style={{ color: brand.coral }}>{cat}</div>
                      <div className="space-y-2">
                        {items.map(item=> (
                          <label key={item.id} className="flex items-center gap-2 text-sm">
                            <Checkbox checked={item.done} onCheckedChange={(val)=>setWeeks(prev=>{const copy=[...prev]; const w={...copy[active]}; w.days=w.days.map((dd,idx)=> idx!==selectedDay?dd:{...dd, tasks:(dd.tasks||[]).map(x=> x.id===item.id?{...x,done:Boolean(val)}:x)}); copy[active]=w; return copy;})} />
                            <span>{item.label}</span>
                          </label>
                        ))}
                      </div>
                    </div>
                  ));
                })()}
              </div>
            </ScallopCard>
          </TabsContent>

          {/* Weekly */}
          <TabsContent value="weekly" className="mt-2">
            <ScallopCard title="Weekly Goals">
              <div className="space-y-2">
                {week.goals.map((g,gi)=>(
                  <label key={g.id} className="flex items-center gap-2 text-sm">
                    <Checkbox checked={g.done} onCheckedChange={(val)=>setWeeks(prev=>{const copy=[...prev]; const w={...copy[active]}; w.goals=w.goals.map((gg,idx)=> idx===gi?{...gg,done:Boolean(val)}:gg); copy[active]=w; return copy;})} />
                    <span>{g.label}</span>
                  </label>
                ))}
              </div>
            </ScallopCard>
          </TabsContent>

          {/* Notes */}
          <TabsContent value="notes" className="mt-2 space-y-4">
            <ScallopCard title="Weight Progress">
              <div className="grid grid-cols-3 gap-2 text-sm">
                <div><div className="text-xs" style={{ color: "#6b7280" }}>Start</div><Input type="number" value={weight.start} onChange={e=>setWeight({...weight,start:Number(e.target.value)})} /></div>
                <div><div className="text-xs" style={{ color: "#6b7280" }}>Current</div><Input type="number" value={weight.current} onChange={e=>setWeight({...weight,current:Number(e.target.value)})} /></div>
                <div><div className="text-xs" style={{ color: "#6b7280" }}>Goal</div><Input type="number" value={weight.goal} onChange={e=>setWeight({...weight,goal:Number(e.target.value)})} /></div>
              </div>
              <Progress value={weightPct} />
              <div className="text-xs" style={{ color: "#475569" }}>{weightPct.toFixed(0)}% toward goal</div>
            </ScallopCard>

            <ScallopCard title="Niche Statement">
              <Textarea value={niche} onChange={e=>setNiche(e.target.value)} />
              <div className="text-xs" style={{ color: "#6b7280" }}>Keep it one sentence. Build assets that prove it.</div>
            </ScallopCard>
          </TabsContent>
        </Tabs>

        {/* Dev tests */}
        <Card className="shadow-sm">
          <CardHeader className="pb-2"><CardTitle className="text-base flex items-center gap-2"><Bug className="w-4 h-4"/> Tests</CardTitle></CardHeader>
          <CardContent className="space-y-2 text-sm">
            <div className="flex items-center gap-2"><Switch checked={showTests} onCheckedChange={setShowTests} /><span>Show tests</span></div>
            {showTests && (
              <ul className="list-disc pl-5 space-y-1">
                {runSmokeTests().map((t,idx)=>(<li key={idx} className={t.pass?"text-emerald-600":"text-rose-600"}><span className="font-medium">{t.name}:</span> {t.pass?"PASS":"FAIL"}{t.detail!==undefined && <span className="text-slate-500"> — {String(t.detail)}</span>}</li>))}
              </ul>
            )}
          </CardContent>
        </Card>

        <div className="text-center text-xs pb-6" style={{ color: "#6b7280" }}>Built for Taylor • Your receipts engine. Progress &gt; perfection.</div>
      </div>
    </PhoneShell>
  );
}
