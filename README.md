// Folder structure: // - auto-cuan-site/ //   - pages/ //     - index.js //     - dashboard.js //     - blog.js //     - api/ //       - register.js //       - login.js //     - _document.js //   - components/ //     - Layout.js //   - lib/ //     - db.js //   - styles/ //     - globals.css //   - .env.local //   - package.json

// --- STEP 1: Basic Next.js Setup ---

// 1. Init Project // > npx create-next-app@latest auto-cuan-site // > cd auto-cuan-site

// 2. Install dependencies // > npm install bcryptjs jsonwebtoken mongoose

// 3. Create .env.local (for secret keys & DB URL)

// --- .env.local --- // MONGO_URI=mongodb+srv://user:password@cluster.mongodb.net/auto-cuan // JWT_SECRET=cuanbanget123

// --- lib/db.js --- import mongoose from 'mongoose';

export async function connectDB() { if (mongoose.connections[0].readyState) return; await mongoose.connect(process.env.MONGO_URI); }

// --- models/User.js --- import mongoose from 'mongoose';

const UserSchema = new mongoose.Schema({ email: String, password: String, referralCode: String, referredBy: String, });

export default mongoose.models.User || mongoose.model('User', UserSchema);

// --- pages/api/register.js --- import { connectDB } from '@/lib/db'; import User from '@/models/User'; import bcrypt from 'bcryptjs';

export default async function handler(req, res) { if (req.method !== 'POST') return res.status(405).end(); await connectDB();

const { email, password, referredBy } = req.body; const hashed = await bcrypt.hash(password, 10); const referralCode = Math.random().toString(36).substring(2, 8);

const user = await User.create({ email, password: hashed, referralCode, referredBy }); res.json({ success: true, user }); }

// --- pages/index.js --- export default function Home() { return ( <div className="p-10"> <h1 className="text-3xl font-bold">Auto Cuan Website</h1> <p>Gabung sekarang dan mulai dapet cuan sambil rebahan.</p> <a href="/dashboard" className="text-blue-600 underline">Masuk Dashboard</a> </div> ); }

// --- pages/dashboard.js --- export default function Dashboard() { return ( <div className="p-10"> <h2 className="text-xl">Dashboard Cuan Kamu</h2> <p>Saldo: Rp0</p> <p>Referral Kamu: auto-cuan.site/ref/ABC123</p> </div> ); }

// --- pages/blog.js --- export default function Blog() { return ( <div className="p-10"> <h2 className="text-2xl font-semibold">Blog Cuan</h2> <p>Selamat datang di blog kami. Di sini kamu bisa belajar cara dapetin cuan dari internet sambil rebahan.</p>

{/* Slot Iklan */}
  <ins className="adsbygoogle"
    style={{ display: 'block', textAlign: 'center' }}
    data-ad-client="ca-pub-8284214804162335"
    data-ad-slot="1234567890"
    data-ad-format="auto"
    data-full-width-responsive="true"></ins>
  <script>(adsbygoogle = window.adsbygoogle || []).push({});</script>
</div>

); }

// --- pages/_document.js --- import { Html, Head, Main, NextScript } from 'next/document';

export default function Document() { return ( <Html> <Head> {/* Google AdSense */} <script
async
src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-8284214804162335"
crossOrigin="anonymous"
></script> </Head> <body> <Main /> <NextScript /> </body> </Html> ); }

// --- styles/globals.css --- body { font-family: sans-serif; background: #f4f4f4; }

// DONE: Struktur dasar + blog + AdSense siap // Next step: Login JWT + referral tracking + konten SEO massal

