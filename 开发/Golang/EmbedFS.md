EmbedFS
=======

```text

web_server
  - static/
     - index.html
     - style.css
  - static.go
  - server.go
```

static.go
```go
package web_server

import "embed"

//go:embed static
var static embed.FS
```

server.go
```go
package web_server

var listen net.Listener

func Start() error {
	var err error
	listen, err = net.Listen("tcp", "0.0.0.0:8080")
	if err != nil {
		return err
	}
	mux := http.NewServeMux()
	mux.HandleFunc("/", root)
	mux.HandleFunc("/vue.js", vueJs)
	mux.HandleFunc("/vue-router.js", vueRouterJs)
	mux.HandleFunc("/allDownloads.json", allDownloads)
	mux.HandleFunc("/downloadResource/", downloadResource)
	mux.HandleFunc("/comic/", comic)
	mux.HandleFunc("/comic/chapters/", comicChapters)
	mux.HandleFunc("/chapter/info/", chapterInfo)
	mux.HandleFunc("/chapter/images/", chapterImages)
	mux.HandleFunc("/next/chapter/", nextChapter)
	go http.Serve(listen, mux)
	return nil
}

func root(w http.ResponseWriter, r *http.Request) {
	if r.RequestURI == "/" {
		file, err := static.Open("static/index.html")
		if err != nil {
			w.WriteHeader(500)
			fmt.Fprint(w, err.Error())
			return
		}
		defer file.Close()
		w.Header().Set("Content-Type", "text/html; charset=utf-8")
		io.Copy(w, file)
		return
	}
	file, _ := static.Open("static" + r.RequestURI)
	if file != nil {
		io.Copy(w, file)
	}
	w.WriteHeader(404)
}
```

