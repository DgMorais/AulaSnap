# Revisão da política pública — AulaSnap V1.x

Revisão preparada em 12/09/2026 para SEC-09 da tarefa 19. Este relatório é técnico e não integra o texto público da política.

## Localização e escopo

- Projeto: `aulasnap_privacy_policy`, pasta ao lado do aplicativo Flutter `../aulasnap`.
- Implementação: HTML estático com CSS embutido, sem JavaScript, dependências ou pipeline de compilação.
- Arquivo/rota relativa: `privacy-policy.html` / `/privacy-policy.html`, se servido na raiz. Não foi encontrada configuração de hospedagem ou URL pública confirmada. O domínio de App Links do app não comprova a URL da política.
- Arquivos entregues: `privacy-policy.html` atualizado e este `REVISAO_PRIVACIDADE.md` criado.
- A pasta da página não é um repositório Git. Nenhum commit criado; mensagem sugerida ao integrar ao repositório web: `feat: atualizar política de privacidade do AulaSnap`.
- Nenhum comportamento, arquivo ou dependência do aplicativo foi alterado. A árvore Git do app permaneceu limpa.

## Texto anterior e lacunas

A página era do **PlantOn**, descrevendo plantões, locais de trabalho, valores monetários, backup JSON e AdMob. Afirmava armazenamento local e ausência de servidor próprio, mas reconhecia exportação e tratamento por publicidade: não dizia literalmente que nenhum dado saía do aparelho. Era inadequada ao AulaSnap e omitia Drive/OAuth, compartilhamentos por referência, revogação limitada, scanner/SDKs, diagnósticos e estado da IA.

Preservados cores, cartões, tipografia, estrutura responsiva e contato `suporte@dmbsoftware.com.br`. Marca e metadados agora identificam AulaSnap. Foco visível, quebra de textos longos, links sublinhados e hierarquia h1/h2/h3 foram acrescentados ou preservados.

## Conteúdo entregue

| Tema | Como ficou documentado |
| --- | --- |
| Local-first / conta | Conteúdo principalmente no aparelho; caderno sem conta AulaSnap, Conta Google ou backend obrigatório; ações opcionais podem transmitir dados, e SDKs podem tratar informações técnicas. Sem sincronização automática. |
| Dados locais | Espaços, Disciplinas, Aulas, assuntos/conexões, notas/Markdown, fotos/imagens, PDFs/materiais, metadados, favoritos, revisão e preferências. SQLite e diretórios privados; preferências pessoais fora dos pacotes atuais. |
| Google Sign-In | Opcional no Android para Drive; identificação por e-mail/nome/foto quando disponíveis; sessão e autorização pelo SDK; senha não recebida ou armazenada pelo AulaSnap. Único escopo Drive: `drive.file`. |
| Drive / backup | Pacote manual de estudo na conta usada, privado na pasta AulaSnap Backups; criar/listar/baixar/importar/excluir. Favoritos, revisão, preferências e credenciais não entram. Importação aditiva, sem substituição automática. |
| Backup Android | Exclusões configuradas; não usado como mecanismo de backup do estudo; sem garantia absoluta sobre fabricantes. |
| Compartilhamento | Pacote selecionado no Drive e referência em link/QR/App Link. Quem obtém referência válida pode acessar; destinatário não usa conta do remetente. |
| Revogação | Impede novos acessos quando aplicada pelo serviço, sem recuperar cópias baixadas/importadas/exportadas. Alterações não sincronizam cópias. |
| Exportação/importação | Destino escolhido pelo usuário/sistema; cópias externas sob práticas do destino; pacotes não criptografados; validação e prévia antes de importar. |
| Câmera/documentos | Captura de estudo e scanner QR; fotos locais salvo inclusão em envios; documentos selecionados copiados para diretório privado; sem acesso amplo ao armazenamento. |
| Internet | OAuth, Drive, recebimento e infraestrutura SDK; dependências consultam conexão; caderno local não depende continuamente de rede. |
| Terceiros | Google Sign-In, Drive, ML Kit, serviços Android e DataTransport; possível processamento/transmissão técnica conforme versão/configuração/política. Sem equiparar ML Kit a Analytics/AdMob ou declarar toda coleta possível como efetiva. |
| Logs | Eventos/categorias sanitizados; sem registro deliberado de senhas, tokens, Authorization, notas integrais, bytes ou credenciais; sem serviço próprio de coleta; diagnósticos de SDK separados. |
| IA/publicidade | IA pública indisponível na V1; nenhum provedor experimental apresentado como ativo; publicidade não disponibilizada na versão atual. Revisão antes de futuras mudanças. |
| Exclusão | Conteúdo local, backup remoto, compartilhamento e cópias externas tratados separadamente; logout/desinstalação não apagam Drive; desinstalação normalmente remove armazenamento privado. |
| Segurança/direitos | Medidas concretas de armazenamento/validação/HTTPS, sem segurança absoluta ou promessa de criptografia; direitos quando aplicáveis e limitação de acesso remoto ao caderno local. |

## Fontes e divergências

Consultados no aplicativo: `docs/security_review_19.md`, `docs/architecture.md`, `docs/google_drive.md`, `docs/google_drive_setup_android.md`, `docs/aulasnap_package_v7.md`, `docs/ai_experimental.md`; manifest Android atual; implementação de escopos/conta/logout em `google_auth_service.dart`; gate `release_features.dart`; logs `security_diagnostics.dart`; tela `data_privacy_screen.dart`. A auditoria documenta o manifest merged release e ML Kit/DataTransport; não foi produzido novo artefato Android nesta tarefa.

1. `data_privacy_screen.dart:93` dizia que **somente backups manuais** eram copiados ao Drive. A rodada de preparação para publicação corrigiu o texto e o teste no repositório do aplicativo para incluir compartilhamentos iniciados pelo usuário, sem alterar o comportamento.
2. A antiga política dizia que o aplicativo não é direcionado a crianças e justificava uso por profissionais de plantões. Na rodada de preparação para publicação, a afirmação ainda não aprovada foi substituída por uma redação neutra: a política não estabelece idade mínima e remete a definição do público-alvo aos canais oficiais de distribuição. Nenhuma idade foi inventada. **PENDÊNCIA DE PRODUTO: definir público-alvo/Target Audience do AulaSnap e validar a seção “Privacidade de crianças” antes da publicação final.**
3. O backup não inclui favoritos/revisão/preferências: a política distingue dados locais do conteúdo efetivamente enviado. “Completo” não é snapshot do aplicativo.
4. Não foi encontrada evidência de publicidade ativa do AulaSnap; a declaração AdMob do PlantOn foi removida.

## Conferência técnica para Data Safety

Esta matriz auxilia a revisão e não determina automaticamente respostas do formulário.

| Fluxo | Dados/destinos | Conferência pendente |
| --- | --- | --- |
| Estudo local | Conteúdo, imagens/PDFs e preferências no aparelho | Distinguir armazenamento local de coleta no formulário. |
| OAuth | Identidade e autorização via infraestrutura Google | Confirmar tela de consentimento, escopo e categorias da identificação. |
| Backup manual | Conteúdo de estudo e arquivos para Drive da conta | Classificar transmissão opcional, finalidade e controles de exclusão. |
| Compartilhamento | Recorte de estudo no Drive, referência para destinatários | Considerar acesso por referência, cópias independentes e tráfego/IP observado pelo provedor. |
| Exportação | Pacote para destino escolhido | Conferir tratamento aplicável a compartilhamento iniciado pelo usuário. |
| ML Kit/DataTransport | Possíveis informações de dispositivo/app/instalação e métricas | Mapear versões resolvidas, configuração real, transmissão, finalidades, retenção e exclusão; não declarar ausência geral de coleta. |
| Logs próprios | Eventos/categorias; sem serviço próprio de coleta | Conferir logcat do release real e separar diagnósticos dos SDKs. |
| IA | Gate público desativado | Reavaliar antes de liberar qualquer integração. |

Fontes oficiais consultadas e links verificados:
- [Política do Google](https://policies.google.com/privacy?hl=pt-BR).
- [Declaração de dados do ML Kit](https://developers.google.com/ml-kit/android-data-disclosure): detalhes dependem da funcionalidade e configuração; não prova que todos os eventos estejam ativos no app.

O Play Console não foi acessado nem modificado. SEC-09 continua dependente da publicação, revisão do responsável e conferência formal dos SDKs/Data Safety.

## Validação executada

- **Build:** não aplicável; a página é o próprio artefato HTML, sem configuração de build. Não atribuir a esta tarefa os builds Flutter da auditoria anterior.
- **Lint:** `npx --yes --package html-validate html-validate privacy-policy.html` (11.15.0), passou sem erros ou avisos. Corrigida notação autocontida das três tags meta apontada na primeira execução. Nenhuma dependência adicionada ao projeto.
- **Testes existentes:** não há suíte/configuração na pasta. Verificação pontual com Playwright/Chromium passou: título, h1 único, data, ausência de PlantOn e erros de página.
- **Responsividade:** larguras 320, 360, 390, 640, 768 e 1280 px sem overflow horizontal; texto a 200% em 640 px sem overflow. Reflow de 320 px cobre largura equivalente a zoom de 400% em 1280 px, mas não é teste de zoom nativo em todos os navegadores. Captura mobile inspecionada; Android físico não testado.
- **Acessibilidade:** hierarquia semântica, links sublinhados, foco visível de teclado verificado no navegador; seis combinações de cores de texto/fundo verificadas com contraste >= 4,5:1. Não equivale a auditoria com leitor de tela.
- **Links:** dois destinos HTTPS responderam HTTP 200; `mailto:` preservado e sintaticamente correto. Não foi enviado e-mail nem confirmada operação da caixa postal.
- **Conteúdo:** sem placeholders ou dados jurídicos inventados. Não foram copiados segredos, tokens ou configurações privadas; os únicos identificadores de acesso apresentados são o escopo público e o contato preexistente.
- **Data:** 12/09/2026 corresponde à revisão e à publicação técnica no GitHub Pages realizada nesta rodada.

## Pendências antes da publicação definitiva

- Confirmar a aplicabilidade do e-mail preservado ao AulaSnap e definir o público-alvo/Target Audience; a política não estabelece idade mínima.
- Responsável jurídico/Produto deve avaliar identificação legal, direitos e adequação do texto. A fonte não trazia nome do responsável/controlador, razão social, CNPJ, endereço, telefone ou DPO; nenhum foi inventado. Definir os dados aplicáveis sem inferir identidade jurídica a partir do domínio do e-mail.
- Conferir Data Safety e comportamento/configuração dos SDKs contra o AAB efetivamente distribuído pelo Google Play, com atenção a Google Sign-In, Drive, ML Kit, DataTransport, identificadores técnicos, diagnósticos, informações de dispositivo/aplicativo e transmissões iniciadas pelo usuário. Testes reais de Drive/revogação/App Links/OEM permanecem os da revisão 19.

## Continuação — versionamento autorizado

Após a revisão inicial, a pasta passou a ter Git configurado com o remoto `https://github.com/DgMorais/AulaSnap.git`, inicialmente sem branches publicados. O usuário autorizou commit e push dos dois arquivos entregues. As observações anteriores sobre ausência de Git descrevem o estado encontrado durante a revisão inicial. O envio ao GitHub não comprova publicação da página em uma hospedagem.

## Rodada de preparação para publicação

### Corrigido

- **Identidade visual:** a cor primária azul `#2563eb` foi substituída pela cor do AulaSnap `#6750E8`; fundo, borda e texto do destaque foram adaptados para a mesma família visual, preservando contraste WCAG AA e foco visível.
- **Texto interno do aplicativo:** a tela Dados e privacidade agora informa que conectar a Conta Google não envia o caderno automaticamente e que tanto backups quanto compartilhamentos podem enviar conteúdo ao Google Drive somente por ação explícita. O aviso de exclusão também esclarece que backups e compartilhamentos remotos permanecem no Drive.
- **Hospedagem estática:** adicionados `.nojekyll` e `index.html`. O índice fornece link acessível e redirecionamento estático para `privacy-policy.html`, sem JavaScript, framework ou dependência. O GitHub Pages foi ativado na branch `master`, raiz `/`, com HTTPS obrigatório.
- **Consistência:** política e aplicativo descrevem estudo local por padrão, Drive opcional, ausência de sincronização automática e independência entre dados locais e cópias remotas.
- **Privacidade de crianças:** removida a afirmação não aprovada de que o aplicativo não é direcionado a crianças. A redação pública ficou neutra e não cria idade mínima.

### Ainda pendente

- **PENDÊNCIA DE PRODUTO:** definir público-alvo/Target Audience do AulaSnap e validar a seção “Privacidade de crianças”.
- **PENDÊNCIA DE PRODUTO/JURÍDICA:** definir quais dados de identificação do responsável/controlador devem constar na política pública. O e-mail preexistente `suporte@dmbsoftware.com.br` foi mantido; razão social, CNPJ, endereço, telefone, pessoa física e DPO não foram inventados.
- Fazer a conferência final do Data Safety contra o AAB efetivamente distribuído pelo Google Play. A matriz técnica deste relatório auxilia a revisão, mas não preenche nem determina respostas do formulário.
- Obter revisão do responsável e revisão jurídica antes da publicação definitiva. Enquanto essas dependências externas permanecerem abertas, **SEC-09 não está totalmente resolvido**.

### Validação desta rodada

- `html-validate 11.15.0` passou em `privacy-policy.html` e `index.html`, sem erros ou avisos.
- Chromium/Playwright confirmou título, `h1`, ausência de erros de página, foco de teclado visível, redirecionamento do índice e ausência de overflow horizontal em 320, 360, 390, 640, 768 e 1280 px; texto a 200% também permaneceu sem overflow.
- As seis combinações de texto/fundo verificadas ficaram entre 4,70:1 e 16,27:1, atendendo ao contraste WCAG AA para texto normal.
- Os dois links HTTPS externos responderam HTTP 200. O link de e-mail foi preservado e não foi acionado.
- O GitHub Pages informou estado `built`, HTTPS obrigatório e origem `master`/`/`. A URL raiz e `https://dgmorais.github.io/AulaSnap/privacy-policy.html` responderam HTTP 200; Chromium confirmou o redirecionamento público, título, `h1`, ausência de PlantOn e a cor primária `#6750E8` no artefato publicado.
- O HTML não contém `PlantOn`, placeholders, `TODO` ou a antiga cor `#2563eb`.
- No aplicativo, `flutter analyze` passou sem problemas e `flutter test test/data_privacy_test.dart` passou com 16 testes. O teste foi atualizado para validar o novo texto e para rolar até o botão de exclusão em vez de depender da altura anterior do conteúdo.
- `git diff --check` passou nos dois repositórios. Nenhum segredo, keystore, credencial OAuth ou arquivo local foi incluído nas alterações.
- Não existe etapa de build para o site estático. O aplicativo não foi compilado porque a alteração é apenas textual; análise e teste de widget cobriram o escopo alterado.
