# DocApp ERP: Sistema de Gestão de Documentação Técnica e Compliance

O **DocApp ERP** é um motor de rastreabilidade documental desenhado para conectar dados operacionais de um ERP às exigências de normas regulamentares (ISO 9001, ISO 14001, ABNT, Portarias Ministeriais e Instruções Normativas). O sistema organiza normas, procedimentos operacionais padrão (POPs), instruções de trabalho (ITs) e laudos de perícia em uma estrutura em árvore totalmente navegável e auditável.

---

## 1. Matriz Mestra de Rastreabilidade Operacional

```text
[Legislação / Norma ABNT]
│
├──► [ISO 9001 / ISO 14001]
│    │
│    ├──► [POP: Procedimento Operacional Padrão]
│    │    │
│    │    └──► [IT: Instrução de Trabalho Técnica]
│    │         │
│    │         └──► [Módulo ERP / Registros de Campo]
```

---

## 2. Árvore de Navegação Documental

### Seção A: Perícia Ambiental e Gestão de Resíduos (ISO 14001)
- **Legislação de Ancoragem**: PNRS - Lei nº 12.305/2010 | ABNT NBR 10004 (Classificação de Resíduos).
- **Procedimento Mestre**: `POP-AMB-001` — Gestão e Destinação de Efluentes de Usinagem e Óleos Solúveis.
- **Instrução de Trabalho**: `IT-AMB-012` — Operação da Caixa Separadora de Água e Óleo (SAO).
- **Registro ERP Vinculado**: `Módulo Estoque/Descarte -> Endpoint /api/v1/residuos/manifesto`.
- **Status de Auditoria**: `[APROVADO]` | **Validade**: 2027-08-30.

### Seção B: Perícia Mecânica e Garantia da Qualidade (ISO 9001)
- **Legislação de Ancoragem**: ABNT NBR 15831 (Retífica de Motores a Combustão Interna).
- **Procedimento Mestre**: `POP-MEC-004` — Controle Metrológico de Blocos e Virabrequins.
- **Instrução de Trabalho**: `IT-MEC-002` — Calibração de Micrômetros e Subitos de Precisão.
- **Registro ERP Vinculado**: `Módulo Ordem de Serviço -> Endpoint /api/v1/os/medicoes-mecanicas`.
- **Status de Auditoria**: `[EM REVISÃO]` | **Validade**: 2026-12-15.

---

## 3. Pipeline de Compilação e Exportação Documental

O DocApp lê os arquivos brutos em Markdown estruturado com metadados YAML e gera pacotes de conformidade exportáveis em PDF/DOCX organizados para auditorias externas:

```bash
# Exemplo de comando de compilação via CLI do projeto
docapp compile --scope=ISO14001 --output=pdf --attach-logs --sign-hash
```

Esquema do Metadado de Documento (frontmatter.json):

```json
{
  "doc_id": "POP-MEC-004",
  "titulo": "Metrologia e Usinagem de Precisão",
  "versao": "2.1.0",
  "autor": "Luiz Ferreira (Mecatrônica/Perícia)",
  "normas_relacionadas": ["ABNT NBR 15831", "ISO 9001:2015 - Cap. 8.5"],
  "its_associadas": ["IT-MEC-002", "IT-MEC-005"],
  "erp_hook": "https://api.retificapro.com.br/v1/quality/check"
}
```

---

## 4. Roteiro de Implementação no ERP

- **Mapeamento de Entidades**: Indexação de todas as portarias, leis e seções ABNT aplicáveis ao negócio.
- **Encadeamento de Dependências**: Vinculação de cada IT ao seu POP correspondente.
- **Automação de Checklists**: Leitura via interface web/mobile com atualização imediata no painel principal do ERP.
- **Emissão de Relatórios**: Geração automática da Lista Mestra com status de conformidade em tempo real.
