# Esquina-cul-
# Next.js Blog (Markdown + Tailwind)

Este es un blog profesional hecho con **Next.js 14** y **Tailwind CSS**, con posts escritos en **Markdown**.

## 🚀 Instalación local

1. Clona el repositorio o crea un nuevo proyecto:
   ```bash
   git clone https://github.com/tuusuario/nextjs-blog.git
   cd nextjs-blog
   ```

2. Instala dependencias:
   ```bash
   npm install
   ```

3. Inicia el servidor de desarrollo:
   ```bash
   npm run dev
   ```

4. Abre en tu navegador:
   ```
   http://localhost:3000
   ```

---

## 📂 Estructura
```
my-blog/
├─ pages/
│  ├─ index.jsx        -> Home (lista de posts)
│  ├─ [slug].jsx       -> Página de cada post
│  ├─ about.jsx        -> Página “Sobre mí”
├─ posts/
│  ├─ bienvenido.md    -> Primer post
├─ components/
│  ├─ Layout.jsx
│  ├─ PostCard.jsx
├─ lib/
│  └─ posts.js         -> Funciones para leer Markdown
├─ styles/
│  └─ globals.css
├─ package.json
```

---

## 📝 Crear un post

1. Entra en la carpeta `/posts/`.
2. Crea un archivo nuevo con extensión `.md` (no `.mdx`):
   ```md
   ---
   title: "Mi segundo post"
   date: "2025-09-25"
   excerpt: "Un resumen corto del post."
   ---

   # Mi segundo post

   Aquí puedes escribir en **Markdown**.
   ```

3. Al guardar, el post se mostrará automáticamente en la página principal.

---

## 🌐 Despliegue en Vercel (gratis)

1. Sube tu proyecto a GitHub.
2. Ve a [Vercel](https://vercel.com/), crea una cuenta y conecta tu GitHub.
3. Importa el repositorio → clic en **Deploy**.
4. Tu blog estará online en `https://tublog.vercel.app` 🎉

---

## 🔧 Notas importantes
- Se cambió `.mdx` a `.md` para evitar errores de compilación si no usas un plugin adicional de MDX.
- Los posts usan **gray-matter** para leer el frontmatter (título, fecha, extracto).
- El contenido se transforma con `remark` y `remark-html`.

---

## ✅ Dependencias necesarias

```json
{
  "dependencies": {
    "gray-matter": "^4.0.3",
    "next": "14.2.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "remark": "^15.0.1",
    "remark-html": "^16.0.1",
    "tailwindcss": "^3.4.0"
  }
}
```

---

Con esto tu blog funcionará sin errores ✅ y podrás **hostearlo gratis en Vercel**.
