# Woliver REST API

<!-- TOC depthFrom:1 depthTo:2 withLinks:1 updateOnSave:1 orderedList:0 -->

  - [Introdução](#introdução)
  - [Autenticação](#autenticação)
  - [Recomendações](#recomendações)
  - [Leads](#leads)
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
| `comments` | `string` | Comentários do lead a respeito do imóvel |

```javascript
{
  "full_name": "Lead Woliver",
  "cpf": "11111111111",
  "email": "dev@woliver.com",
  "phone_number": "(99)99999999",
  "listing": 1532,
  "comments": "Quero agendar uma visita!"
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
        "leads_source": null
    },
]
```

###  Detalhes da lead

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
    "leads_source": null
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
            "origin": 5
        },
    ],
    "hide_filters": false,
    "filters": {},
    "name": null
}
```

## Agendamentos

### Criar novo agendamento

Esse endpoint é utilizado para criar um agendamento de visita a um imóvel.

```http
POST /api/v1/bookings/?token=12345678901234567890123456789012
```

#### Query Parameters

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `token` | `string` | **Obrigatório**. Sua Woliver API token |

#### Request

| Parâmetro | Tipo | Descrição |
| :--- | :--- | :--- |
| `listing` | `number` | **Obrigatório**. Código do imóvel de acordo com o XML da imobiliária |
| `date_and_time` | `datetime` | **Obrigatório** Data e horário da visita |
| `user_id` | `number` | **Obrigatório** Código do cliente que deseja agendar a visita |
| `staff_hash` | `string` | Código do corretor responsável pelo agendamento da visita |


```javascript
{
	"listing": "00000",
	"date_and_time": "2020-12-25T15:00:00Z",
	"user_id": 2,
	"staff_hash": "staff"
}
```

#### Response - 201 (application/json)

```javascript
{
    "id": 12,
    "user": {
        "id": 2,
        "phone_number": "+5599999999999",
        "email": "dev@woliver.com",
        "full_name": "Lead Wolier",
        "cpf": "00000000000",
        "date_of_birth": "1996-12-03",
        "rg": "0000000",
        "issuing_agency": "SSP",
        "gender": "",
        "civil_status": null,
        "nationality": "null,
        "occupation": null,
        "professional_data": null,
        "current_address": "Address",
        "role": "client",
        "backoffice_filters": null,
        "need_change_password": false,
        "picture": null,
        "active_lease": null
    },
    "staff": {
        "id": 5,
        "phone_number": null,
        "email": "",
        "full_name": "Staff",
        "cpf": null,
        "date_of_birth": null,
        "rg": null,
        "issuing_agency": null,
        "gender": null,
        "civil_status": null,
        "nationality": null,
        "occupation": null,
        "professional_data": null,
        "current_address": null,
        "role": "staff",
        "backoffice_filters": null,
        "need_change_password": false,
        "picture": null,
        "active_lease": null
    },
    "lease": {
        "id": 2,
        "rent_price": 10000.0,
        "total_price": 10858.0,
        "status": "booking_scheduled",
        "monthly_income": null,
        "total_monthly_income": 0,
        "created_at": "2020-07-09T20:45:06.520186Z",
        "updated_at": "2020-07-09T21:23:32.599802Z",
        "income_approved": null,
        "identification_approved": null,
        "proof_of_income_approved": null,
        "credit_card_statement_approved": null,
        "required_income": 54290.0,
        "proposal": null,
        "warranty": "rapid",
        "fire_insurance_price": null,
        "fire_insurance_agreed": null
    },
    "phone_number": "+5599999999999",
    "available": true,
    "created_at": "2020-07-31T20:43:26.011032Z",
    "updated_at": "2020-07-31T20:43:26.011101Z",
    "date_and_time": "2020-12-25T15:00:00Z",
    "warned": false,
    "cancelled": false,
    "booking_extra_info": null,
    "booking_key_option": null,
    "listing": {
        "id": 4,
        "external_id": 00000,
        "title": null,
        "address": "RUA",
        "street_number": "0",
        "neighborhood": "BAIRRO",
        "city": "CIDADE",
        "converted_pictures": [],
        "rent_price": 10000.0,
        "condominium_fee": 580.0,
        "taxes": 278.0,
        "insurance": null,
        "total_price": 10858.0,
        "bedrooms": 1,
        "listing_type": "res_home",
        "interested_people": 0,
        "reserved": false,
        "contract_signed": false
    }
}
```