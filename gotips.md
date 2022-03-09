# Go

https://go.dev/

## FUSE (Frequent Use)

* [String Formating](https://pkg.go.dev/fmt)
* [Date & Time Formating](https://pkg.go.dev/time#pkg-constants)

## Program Struture

- FR [Initiation aux modules](https://dpp.st/blog/golang-modules/)

## Build & co

```bash
# build for another platform eg Raspi
GOOS=linux GOARCH=arm GOARM=5 go build;

# runing full tests
go test ./...

```

- [how to deploy step by step](https://codesahara.com/blog/how-to-deploy-golang-to-production-step-by-step/)

## Dev
- go.dev [Language Specification](https://go.dev/ref/spec)
- [Array vs Slice vs Map](http://donofden.com/blog/2019/09/20/golang-array-slice-map)
- [Closures and anonymous](https://www.bogotobogo.com/GoLang/GoLang_Closures_Anonymous_Functions.php)
- [Implementing Enums in Golang](https://levelup.gitconnected.com/implementing-enums-in-golang-9537c433d6e2)

### packages

* [CoinGecko API](https://www.coingecko.com/en/api)
* [CoinGecko API Go Package](https://github.com/superoo7/go-gecko)


### clear a map

```go
for k := range m {
    delete(m, k)
}
```

### Tuto & Examples

- [Writing a Go client for your RESTful API](https://medium.com/@marcus.olsson/writing-a-go-client-for-your-restful-api-c193a2f4998c)
- [BogoToBogo](https://www.bogotobogo.com/GoLang/GoLang_HelloWorld.php)
- [Go By Example](https://gobyexample.com/)
