# QA Newman Com GitHub Actions e GitHubP Pages
  
## Pipeline de QA para testar retorno de API utilizando Newman, Github Actions e Github Pages.  
  

### O [endpoint](https://servicodados.ibge.gov.br/api/docs/nomes?versao=2) escolhido retorna os 20 nomes mais comuns no Brasil cujo o retorno possui o seguinte padrão:  
```console
[
    {
        "localidade": "string",
        "sexo": "string",
        "res": [
            {
                "nome": "string",
                "frequencia": number,
                "ranking": number
            }
        ]
    }
]
```  
  
### São realizados 3 testes básicos na resposta recebida:
- Código de resposta igual a 200
- Resposta menor que N segundos
- Ter no corpo da resposta um conteúdo específico  
  
### A criação do relatório é realizada utilizando a imagem [dannydainton/htmlextra](https://github.com/DannyDainton/newman-reporter-htmlextra).  
```console
          docker container run -t -v $(pwd):/etc/newman dannydainton/htmlextra \
          run 'qa-newman-actions-pages.json' \
          -k \
          -r cli,htmlextra \
          --reporter-htmlextra-export /etc/newman/docs/index.html \
          --reporter-htmlextra-browserTitle "QA Newman" \
          --reporter-htmlextra-title "QA Newman - HTMLEXTRA, GitHub Actions e GitHub Pages"
```  
  
### Etapa de publicação no GitHub Pages. Utilizamos o [GitHub Pages Action](https://github.com/marketplace/actions/github-pages-action) para isso.
```console
      - name: Publish Report
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs
```

Concluído o pipeline com sucesso, podemos ver o relatório [**aqui**](https://consultoria-sensedia.github.io/qa-newman-actions-pages/).
  

Curtam esse pipeline =)