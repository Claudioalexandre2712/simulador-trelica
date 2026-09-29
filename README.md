# Simulador de treliça

Simulador interativo de uma treliça plana 2D. É uma página estática única (`index.html`), sem servidor, sem dependências externas e sem coleta de dados de quem acessa.

## Publicar (GitHub Pages)

1. No repositório: **Settings → Pages**.
2. Em *Source*, escolha **Deploy from a branch**, branch `main`, pasta `/ (root)`, e salve.
3. O site fica em `https://claudioalexandre2712.github.io/simulador-trelica/`.

## Publicar (Vercel)

1. Em [vercel.com](https://vercel.com), entre com o GitHub e dê acesso só a este repositório (*Only select repositories*).
2. **Add New → Project**, importe `simulador-trelica`, deixe *Framework Preset* em **Other** e clique em **Deploy**.
3. Cada push no `main` atualiza o site. Para divulgar, use o link de *Production*, não os de *Preview*.

O `vercel.json` adiciona cabeçalhos de segurança que o GitHub Pages não permite configurar (bloqueio de moldura/iframe, `nosniff`, HSTS, bloqueio de câmera, microfone e localização).

## Prévia do link (LinkedIn, WhatsApp etc.)

As tags `og:*` no `<head>` definem título, descrição e a imagem `og.png` (1200×630) que aparecem ao compartilhar o link. A imagem é servida pelo GitHub Pages, então vale para qualquer endereço do site. Se o LinkedIn mostrar uma prévia antiga, atualize em [linkedin.com/post-inspector](https://www.linkedin.com/post-inspector/).

## Segurança

O `<head>` do `index.html` define (vale em qualquer hospedagem):

- **Content-Security-Policy**: o navegador só executa o script da própria página (liberado por hash SHA-256) e bloqueia qualquer script, conexão, formulário ou recurso externo.
- **Referrer Policy `no-referrer`**: links não enviam o endereço da página para outros sites.

**Ao editar o código dentro de `<script>`**, o hash deixa de bater e o navegador bloqueia o script (a página abre sem a treliça). Para recalcular o hash e atualizar a CSP, rode na pasta do projeto:

```sh
python3 - <<'EOF'
import re, hashlib, base64
s = open('index.html', encoding='utf-8').read()
js = re.search(r'<script>(.*?)</script>', s, re.S).group(1)
h = base64.b64encode(hashlib.sha256(js.encode('utf-8')).digest()).decode()
s = re.sub(r"script-src 'sha256-[^']+'", f"script-src 'sha256-{h}'", s)
open('index.html', 'w', encoding='utf-8').write(s)
print('hash atualizado:', h)
EOF
```

Mudanças só no HTML ou no CSS não exigem esse passo.

Feito por Claudio Alexandre.
