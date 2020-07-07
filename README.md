# Woliver REST API

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

  - [Introdução](#introdução)
  - [Autenticação](#autenticação)
  - [Leads](#leads)
  - [Imóveis](#imóveis)
  - [Visitas](#visitas)
  - [Recomendações](#recomendações)

<!-- /TOC -->

## Introdução

A Woliver é uma empresa dedicada a democratização ao acesso a tecnologias com foco em Real Estate.

Essa documentação possui o manual da API Woliver que pode ser utilizada por desenvolvedores para integrações com nossas tecnologias.

Todas as integrações são realizadas com uma imobiliária e a **BASE URL** da API segue o padrão:

```http
example.api.woliver.net
```

Referente a imobiliária de URL: [example.woliver.net](https://example.woliver.net). A propósito, para questões de documentação usaremos a imobiliária **example** nesse manual.


Esperamos que você goste da API Woliver, mas não hesite em [criar uma issue](https://github.com/woliverhq/api/issues/new) se houver algo faltando.

## Autenticação

Todos os chamados de API exigem uma API token. A API token deve ser requisitada para equipe Woliver e é única para cada imobiliária.

```http
GET /api/v1/example/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

## Leads

### Criar lead

Esse endpoint é utilizado para a criação de novas leads. O usuário deverá prover os parâmetros listados a seguir.

```http
POST /api/v1/leads/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` |**Obrigatório.** Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `full_name` | `string` |**Obrigatório.** Nome completo do lead |
| `cpf` | `string` | CPF do lead (apenas números) |
| `email` | `string` |**Obrigatório.** E-mail do lead |
| `phone_number` | `string` | Número de telefone do lead com DDD |
| `listing` | `number` |**Obrigatório.** Código do imóvel de acordo com o XML da imobiliária |
| `source` | `slug` |Slug name da origem da lead |
| `source_id` | `number` |Código da origem da lead |
| `comments` | `string` | Comentários do lead a respeito do imóvel |

```javascript
{
  "full_name": "Lead Woliver",
  "cpf": "11111111111",
  "email": "dev@woliver.com",
  "phone_number": "+554899999999",
  "listing": "75761",
  "source": "canaldigital",
  "source_id": 8,
  "comments": "Quero agendar uma visita!"
}
```
#### Response - 201 (application/json)

```javascript
{
    "id": 257,
    "user": {
        "id": 92,
        "phone_number": "+554899999999",
        "full_name": "Lead Woliver",
        "email": "dev@woliver.com"
    },
    "listing": "75761",
    "staff1": null,
    "source_id": 8
}
```

###  Listar leads

Esse endpoint é utilizado para listar todas as leads existentes. O usuário deverá prover os parâmetros listados a seguir.

```http
GET /api/v1/leads/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` |**Obrigatório.** Sua Woliver API token |

#### Response - 200 (application/json)

```javascript
[
    {
        "id": 864,
        "user": {
            "id": 9,
            "phone_number": "+5599999999999",
            "full_name": "Lead Woliver",
            "email": "dev@jungledevs.com"
        },
        "listing": "Listing 1532 (1532)",
        "staff1": null,
        "source_id": null
    },
]
```

### Detalhes da lead

Esse endpoint é utilizado para acessar os detalhes de uma lead específica. O usuário deverá prover os parâmetros listados a seguir.

```http
GET /api/v1/leads/{id}/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` |**Obrigatório.** Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `id` | `string` | **Obrigatório**. Id da lead |

#### Response - 200 (application/json)

```javascript
{
    "id": 864,
    "user": {
        "id": 9,
        "phone_number": "+5599999999999",
        "full_name": "Lead Woliver",
        "email": "dev@jungledevs.com"
    },
    "listing": "Listing 1532 (1532)",
    "staff1": null,
    "source_id": null
}
```

###  Listar origem das leads

Esse endpoint é utilizado para acessar a lista cadastrada na Woliver de origem das leads

```http
GET /api/v1/leads-sources/?token=12345678901234567890123456789012
```

#### Response - 200 (application/json)

```javascript
[
    {
        "id": 1,
        "slug_name": "hints",
        "name": "hints.com.br"
    },
    {
        "id": 2,
        "slug_name": "ayla",
        "name": "Site Ayla"
    },
    {
        "id": 3,
        "slug_name": "olx",
        "name": "OLX"
    },
    {
        "id": 4,
        "slug_name": "mercadolivre",
        "name": "Mercado Livre"
    },
    {
        "id": 5,
        "slug_name": "chavenamao",
        "name": "Chave na mão"
    },
    {
        "id": 6,
        "slug_name": "imovelweb",
        "name": "Imovel Web"
    },
    {
        "id": 7,
        "slug_name": "zapimoveis",
        "name": "Zap Imóveis"
    }
    {
        "id": 8,
        "slug_name": "canaldigital",
        "name": "Canal Digital"
    },
    {
        "id": 9,
        "slug_name": "vivareal",
        "name": "Viva Real"
    },
     {
        "id": 10,
        "slug_name": "website",
        "name": "Website"
    },
    {
        "id": 11,
        "slug_name": "whatsapp",
        "name": "WhatsApp"
    },

]
```

#### Origem das leads cadastradas na Woliver

| Id | Nome | Slug |
| :--- | :--- | :--- |
| 1 | hints.com.br | hints |
| 2 | Site Ayla | ayla |
| 3 | OLX | olx |
| 4 | Mercado Livre | mercadolivre |
| 5 | Chave na mão | chavenamao |
| 6 | Imovel Web | imovelweb |
| 7 | Zap Imóveis | zapimoveis |
| 8 | Canal Digital | canaldigital |
| 9 | Viva Real | vivareal |
| 10 | Website | website |
| 11 | WhatsApp | whatsapp |

## Imóveis

### Listar imóveis

Esse endpoint é utilizado para obter a lista de imóveis disponíves da imobiliária.

```http
GET /api/v1/listings/?token=12345678901234567890123456789012
```

#### Query Parameters
| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |
| `price` | `string` | Preço para aluguel do imóvel |
| `taxes_included` | `boolean` | Considerar valores de IPTU e condomínio no valor de aluguel |
| `city` | `string` | Cidades para busca de imóveis |
| `neighborhoods` | `string` | Bairros para busca de imóveis |
| `bathrooms` | `number` | Número de banheiros do imóvel |
| `bedrooms` | `number` | Número de quartos do imóvel |
| `parking_spots` | `number` | Número de garagens do imóvel |
| `furnished` | `string` | Tipo de mobilia: "furnished", "semi_furnished", "unfurnished" |
| `pets` | `boolean` | Imóvel permite pets ou não |
| `listing_type` | `string` | Tipo de imóvel: Casa e/ou Apartamento |

Os parâmetros opcionais, quando utilizados, serão levados em conta na atribuição de `scores` aos imóveis, gerando uma
pesquisa personalizada.

#### Response - 200 (application/json)

```javascript
{
    "count": 667,
    "next": "https://example.api.woliver.net/api/v1/listings/",
    "previous": null,
    "exact": 122,
    "results": [
        {
            "id": 894,
            "liked": null,
            "disliked": null,
            "score": 100.0,
            "exact": true,
            "distance": null,
            "booking": null,
            "coordinates": {
                "type": "Point",
                "coordinates": []
            },
            "converted_pictures": [
                {
                    "picture": "https://picture-url.jpg",
                    "thumbnail": "https://thumbnail-url.jpg",
                    "picture_800x600": "picture-url-800x600.jpg"
                },
            ],
            "pictures": [
                "https://picture-1-url.jpg",
            ],
            "reserved": false,
            "external_id": "00000",
            "title": "TÍTULO DO ANÚNCIO",
            "url": "https://www.example.com.br/codigo/16430",
            "video": null,
            "address": "RUA",
            "extra_address_info": null,
            "street_number": "000",
            "postal_code": "00000-000",
            "neighborhood": "BAIRRO",
            "city": "CIDADE",
            "neighborhood_slug": "bairro",
            "city_slug": "cidade",
            "state": "UF",
            "listing_type": "res_apartment",
            "purpose_type": "residential",
            "transaction_type": "rent",
            "bedrooms": 3,
            "bathrooms": 1.0,
            "number_of_ensuites": 0,
            "parking_spots": 1,
            "living_area": 75.0,
            "total_area": 84.0,
            "furnished": "unfurnished",
            "description": "Descrição do imóvel",
            "listing_description": null,
            "condominium_description": null,
            "rent_price": 1800.0,
            "sale_price": null,
            "condominium_fee": 586.0,
            "taxes": 97.0,
            "insurance": null,
            "available": true,
            "pets": false,
            "amenities": null,
            "incomplete_email_sent": false,
            "invalid": false,
            "contract_signed": false,
            "zone": 5,
            "origin": 17,
            "origin_name": "Brognoli",
        },
    ],
    "hide_filters": false,
    "filters": {},
    "name": null
}
```

## Visitas

### URL para agendamento de visita

Para direcionar o usuário para o agendamento de visita para um imóvel específico, basta utilizar a URL com o seguinte formato:

[example.woliver.net/agendamento/ID_DO_IMOVEL](https://example.woliver.net/agendamento/ID_DO_IMOVEL).

Exemplo, para o imóvel com código 74802 no demo.woliver.net, você pode agendar uma visita acessando https://demo.woliver.net/agendamento/74802.

Essa URL pode ser utilizada no próprio site da imobiliária ou quaisquer ferramentas com a função de direcionar o usuário para o agendamento de visita em um imóvel específico. Por exemplo, adicionando um botão "Agendar Visita" nos detalhes do imóvel no site da imobiliária em si.

### URL para agendamento de visita com identificador da origem

Se você busca entender qual a origem do agendamento de visita em um imóvel, basta adicionar a source_id ou slug na URL de agendamento de visita:

- Opção 1 (source id): https://demo.woliver.net/agendamento/29939?source_id=7

- Opção 2 (source slug): https://demo.woliver.net/agendamento/29939?source=zapimoveis

Os valores possíveis para o campo source podem ser visualizados na tabela acima (#Origem das leads cadastradas na Woliver). Esses valores serão exportados junto com as informações da lead no arquivo dados.csv que pode ser acessado no back office da imobiliária.

## Recomendações

### Criar URL de recomendações - filtros de busca

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis a partir de filtros de busca. Ao abrir a URL a lead irá visualizar uma lista de imóveis organizados de acordo com os parâmetros de criação listados a seguir.

```http
POST /api/v1/recommendations/?token=12345678901234567890123456789012
```
#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `coordinates` | `string` | Coordenadas para buscar imóveis próximos |
| `price` | `string` | Preço para aluguel do imóvel |
| `taxes_included` | `boolean` | Considerar valores de IPTU e condomínio no valor de aluguel |
| `city` | `string` | Cidades para busca de imóveis |
| `neighborhoods` | `string` | Bairros para busca de imóveis |
| `bathrooms` | `number` | Número de banheiros do imóvel |
| `bedrooms` | `number` | Número de quartos do imóvel |
| `parking_spots` | `number` | Número de garagens do imóvel |
| `furnished` | `string` | Tipo de mobilia: "furnished", "semi_furnished", "unfurnished" |
| `pets` | `boolean` | Imóvel permite pets ou não |
| `listing_type` | `string` | Tipo de imóvel: Casa e/ou Apartamento |
| `name` | `string` | Nome da lead |
| `email` | `string` | Email da lead utilizado para enviar recomendações |
| `staff_email`| `string` | Email do corretor responsável pela recomendação |
| `hide_filters` | `boolean` | Esconder os filtros no header da url de recomendações |
| `limit` | `number` | Limite de imóveis a serem retornados |
| `page_size` | `number` | Número de imóveis a serem retornados por página |

```javascript
{
  "coordinates": "[[-27.570903, -48.508389]]",
  "price": 1500.0,
  "city": "florianopolis,sao-jose",
  "neighborhoods": "trindade,itacorubi",
  "bathrooms": 1,
  "bedrooms": 2,
  "parking_spots": 1,
  "furnished": "unfurnished",
  "pets": "true",
  "listing_type": "res_home,res_apartment",
  "nome": "",
  "email": "dev@woliver.com.br",
  "staff_email": "staff@woliver.com.br",
  "hide_filters": false, 
  "limit": 10,
  "page_size": 3
}
```

#### Response - 201 (application/json)

```javascript
{
  "url": "https://example.woliver.net/recomendacoes/k28Jv5?staff_hash=STAFF"
}
```

### Criar URL de recomendações - código do imóvel

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis a partir de um código do imóvel. Ao abrir a URL a lead irá visualizar uma lista de imóveis similares ao imóvel referente ao código passado como parâmetro na url.

```http
POST /api/v1/listings/{listing_id}/recommend/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `listing_id` | `string` | **Obrigatório**. Código do imóvel de acordo com o XML da imobiliária |
| `phone_number` | `string` | Telefone da lead |
| `name` | `string` | Nome da lead |
| `email` | `string` | Email da lead utilizado para enviar recomendações |

```javascript
{
    "phone_number": "+5548999999999"
    "name": "Lead Woliver",
    "email": "dev@woliver.com.br"
}
```

#### Response - 201 (application/json)

```javascript
{
  "url": "https://example.woliver.net/recomendacoes/k28Jv5?staff_hash=STAFF"
}
```
