---
tipo: conceito
tags: [arquitetura, integracao]
---

# Provider

Um provider encapsula o acesso a uma capacidade externa — por exemplo, e-mail, pagamentos, LLM ou armazenamento. Ele existe para concentrar configuração, adaptação de protocolo, falhas e política de retry, evitando espalhar detalhes do fornecedor pelo domínio.

Documente contrato, fornecedor, política de erro, observabilidade e como o provider é substituído em teste. Relacionado: [[Dependency Injection]] · [[Adapter]].
