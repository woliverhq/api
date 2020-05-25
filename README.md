# Woliver REST API

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

  - [Introdução](#introdução)
  - [Autenticação](#autenticação)
  - [Recomendações](#recomendações)
  - [Imóveis](#imóveis)

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

## Recomendações

### Criar URL de recomendações

Esse endpoint é utilizado para a geração de uma URL com recomendações de imóveis. Ao abrir a URL a lead irá visualizar uma lista de imóveis organizados de acordo com os parâmetros de criação listados a seguir.

```http
POST /api/v1/recommendations/?token=12345678901234567890123456789012
```

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |
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
| `hide_filters` | `boolean` | Esconder os filtros no header da url de recomendações |
| `limit` | `number` | Limite de imóveis a serem retornados |
| `page_size` | `number` | Número de imóveis a serem retornados por página |

#### Request

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
  "hide_filters": false, 
  "limit": 10,
  "page_size": 3
}
```

#### Response - 201 (application/json)

```javascript
{
  "url": "https://example.woliver.net/recomendacoes/k28Jv5/"
}
```

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

#### Response - 201 (application/json)

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
            "origin": 5
        },
    ],
    "hide_filters": false,
    "filters": {},
    "name": null
}
```