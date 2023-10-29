# Membuat Pesanan Menggunakan Bearer Token Otentikasi

## Daftar Isi

- [Tools](#Tools)
- [Penggunaan](#penggunaan)
- [Dokumentasi](#dokumentasi)
- [Kode](#kode)

## Tools

Menginstal Tools yang diperlukan:

1. Postman
2. Cypress
3. Visual Studio Code

## Penggunaan

Cara menggunakan proyek ini:

1. Dapatkan Workspace Simple Book API 
2. Aplikasikan ke Postman
3. Manage kode nya Visual Studio Code
4. Cypress Akan mejadi tools utama untuk Pengujian Otomatisnya


## Dokumentasi
[Slide](https://www.canva.com/design/DAFyowLOA4I/Ms8XI1J0uIvw14PhEsZbwA/edit?utm_content=DAFyowLOA4I&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

## Kode

```javascript
// Contoh kode untuk membuat pesanan menggunakan bearer token
describe('API Testing', () => {
    
    let AuthToken = null;

    before('creating access token',() => {
        cy.request({
            method: 'POST',
            url: 'https://simple-books-api.glitch.me/api-clients',
            headers: { 'content-type': 'application/json'},
            body: {
                clientName: "bagus",
                clientEmail: Math.random().toString(5).substring(2)+"@gmail.com"
                }
        }).then((response)=>{
            AuthToken=response.body.accessToken;
        })    
    });

    before('creating new Order',() => {
        cy.request({
            method: 'POST',
            url: 'https://simple-books-api.glitch.me/orders',
            headers: { 'content-type': 'application/json',
                        'Authorization': 'Bearer '+AuthToken //harus spesifik tidak bisa langsung di panggil makanya memakai bearer
                    },
            body: {
                "bookId": 3,
                "customerName": "marco"
                     }
        }).then((response)=>{
            expect(response.status).equal(201);
            expect(response.body.created).to.eq(true)
        })    
    });


    it('Fethcing the Orders', () => {
        cy.request({
            method: 'GET',
            url: 'https://simple-books-api.glitch.me/orders/',
            headers: { 'content-type': 'application/json',
                       'Authorization': 'Bearer '+AuthToken }
        }).then((response)=>{
            expect(response.status).equal(200)
            expect(response.body).has.length(1)
        })
    });


});
// Gantilah dengan kode Anda sendiri
