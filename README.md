# Esquina-cul-
my-blog/
├─ package.json
├─ tailwind.config.js
├─ styles/
│   └─ globals.css
├─ lib/
│   └─ posts.js
├─ components/
│   ├─ Layout.jsx
│   └─ PostCard.jsx
├─ pages/
│   ├─ index.jsx
│   ├─ [slug].jsx
│   └─ about.jsx
└─ posts/
    └─ bienvenido.md
{
  "name": "nextjs-blog",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "gray-matter": "^4.0.3",
    "next": "14.2.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "remark": "^15.0.1",
    "remark-html": "^16.0.1",
    "tailwindcss": "^3.4.0",
    "@tailwindcss/typography": "^0.5.10"
  }
}
module.exports = {
  content: ["./pages/**/*.{js,jsx}", "./components/**/*.{js,jsx}"],
  theme: { extend: {} },
  plugins: [require('@tailwindcss/typography')],
}
@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  @apply bg-gray-50 text-gray-900;
}
import fs from 'fs'
import path from 'path'
import matter from 'gray-matter'

const postsDirectory = path.join(process.cwd(), 'posts')

export function getSortedPostsData() {
  const fileNames = fs.readdirSync(postsDirectory)
  const allPostsData = fileNames.map(fileName => {
    const slug = fileName.replace(/\.md$/, '')
    const fullPath = path.join(postsDirectory, fileName)
    const fileContents = fs.readFileSync(fullPath, 'utf8')
    const matterResult = matter(fileContents)
    return {
      slug,
      ...matterResult.data
    }
  })
  return allPostsData.sort((a, b) => (a.date < b.date ? 1 : -1))
}

export function getAllPostSlugs() {
  const fileNames = fs.readdirSync(postsDirectory)
  return fileNames.map(fileName => ({ params: { slug: fileName.replace(/\.md$/, '') } }))
}

export function getPostData(slug) {
  const fullPath = path.join(postsDirectory, slug + '.md')
  const fileContents = fs.readFileSync(fullPath, 'utf8')
  const matterResult = matter(fileContents)
  return {
    slug,
    ...matterResult.data,
    content: matterResult.content
  }
}
import Link from 'next/link'

export default function Layout({ children }) {
  return (
    <div className="min-h-screen flex flex-col">
      <header className="bg-white shadow">
        <div className="max-w-4xl mx-auto p-4 flex justify-between items-center">
          <Link href="/" className="text-2xl font-bold">Mi Blog</Link>
          <nav className="space-x-4">
            <Link href="/">Inicio</Link>
            <Link href="/about">Sobre</Link>
          </nav>
        </div>
      </header>
      <main className="flex-1 max-w-4xl mx-auto p-4">{children}</main>
      <footer className="bg-gray-100 text-center p-4 text-sm text-gray-600">
        © {new Date().getFullYear()} Mi Blog. Todos los derechos reservados.
      </footer>
    </div>
  )
}
import Link from 'next/link'

export default function PostCard({ post }) {
  return (
    <article className="bg-white rounded-lg shadow p-4">
      <h2 className="text-xl font-semibold mb-1">
        <Link href={`/${post.slug}`}>{post.title}</Link>
      </h2>
      <p className="text-sm text-gray-500">{post.date}</p>
      <p className="mt-2 text-gray-700">{post.excerpt}</p>
    </article>
  )
}
import Layout from '../components/Layout'
import { getSortedPostsData } from '../lib/posts'
import PostCard from '../components/PostCard'

export async function getStaticProps() {
  const allPostsData = getSortedPostsData()
  return { props: { allPostsData } }
}

export default function Home({ allPostsData }) {
  return (
    <Layout>
      <h1 className="text-3xl font-bold mb-6">Últimos Posts</h1>
      <div className="space-y-4">
        {allPostsData.map(post => <PostCard key={post.slug} post={post} />)}
      </div>
    </Layout>
  )
}
import Layout from '../components/Layout'
import { getAllPostSlugs, getPostData } from '../lib/posts'
import { remark } from 'remark'
import html from 'remark-html'

export async function getStaticPaths() {
  const paths = getAllPostSlugs()
  return { paths, fallback: false }
}

export async function getStaticProps({ params }) {
  const postData = getPostData(params.slug)
  const processedContent = await remark().use(html).process(postData.content)
  const contentHtml = processedContent.toString()
  return { props: { postData: { ...postData, contentHtml } } }
}

export default function Post({ postData }) {
  return (
    <Layout>
      <article className="prose prose-lg max-w-none bg-white p-6 rounded-lg shadow">
        <h1>{postData.title}</h1>
        <p className="text-sm text-gray-500">{postData.date}</p>
        <div dangerouslySetInnerHTML={{ __html: postData.contentHtml }} />
      </article>
    </Layout>
  )
}
import Layout from '../components/Layout'

export default function About() {
  return (
    <Layout>
      <div className="prose bg-white p-6 rounded-lg shadow">
        <h1>Sobre mí</h1>
        <p>Este es un blog profesional creado con Next.js y Tailwind, desplegado gratis en Vercel.</p>
      </div>
    </Layout>
  )
}
---
title: "Bienvenido al blog"
date: "2025-09-25"
excerpt: "Este es el primer post de tu nuevo blog profesional."
---

# Bienvenido

Este es un ejemplo de post en **Markdown**. Puedes escribir en formato simple y quedará con estilo profesional. 

- Fácil de editar  
- Gratis de desplegar  
- SEO optimizado  
