# MGRoute

[![License](https://img.shields.io/badge/license-MIT-5351FB)](LICENSE)

MGRoute é uma biblioteca simples e leve para roteamento de URLs em Go, inspirada em abordagens minimalistas e funcionais. Ela permite analisar URLs, extrair partes do caminho e associar padrões de rotas a funções de forma clara e objetiva.

---

## ✨ Recursos

* Parsing de URL
* Extração de partes do caminho (`/users/10/profile` → `["users", "10", "profile"]`)
* Verificação de rotas via expressões regulares
* Execução de handlers baseados em padrões (paradigma funcional)
* Tratamento simples de erro 404
* Código idiomático e compatível com Go moderno

---

## 📦 Instalação

Dentro do seu projeto Go:

```bash
go get github.com/profmugomes/mgroute
```

---

## 🚀 Uso Básico

### Criando uma instância do router

```go
route := mgroute.New("/users/10")
```

---

### Acessando partes da URL

```go
route.GetArrayURLs()      // []string{"users", "10"}
route.GetFirstURL()       // "users"
route.GetLastURL()        // "10"
route.GetURL(1)           // "10"
```

---

### Verificando uma rota

```go
if route.CheckURL(`/users/\d+`) {
    fmt.Println("Rota válida")
}
```

---

### Definindo rotas com handlers

```go
route.GetPart(`/users/(\d+)`, func(args ...string) {
    userID := args[0]
    fmt.Println("User ID:", userID)
})

route.GetError(func() {
    fmt.Println("404 - Not Found")
})
```

---

## 🌐 Exemplo com servidor HTTP

```go
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
    route := mgroute.New(r.URL.Path)

    route.GetPart(`/`, func(args ...string) {
        fmt.Fprintln(w, "Home")
    })

    route.GetPart(`/users/(\d+)`, func(args ...string) {
        fmt.Fprintf(w, "User ID: %s", args[0])
    })

    route.GetError(func() {
        w.WriteHeader(http.StatusNotFound)
        fmt.Fprintln(w, "404 - Not Found")
    })
})

http.ListenAndServe(":8000", nil)
```

---

## 🧩 Compatibilidade

* Go 1.27.2+

---

## 👤 Autor

**Murilo Gomes**

🔗 [https://www.profmugomes.com.br](https://www.profmugomes.com.br)

📺 [https://youtube.com/@profmugomes](https://youtube.com/@profmugomes)

---

## License

Copyright (c) 2026 Murilo Gomes <profmugomes.com.br>

Licensed under the [MIT](https://github.com/profmugomes/mgroute/blob/main/LICENSE) license.

All contributions to the MGRoute are subject to this license.