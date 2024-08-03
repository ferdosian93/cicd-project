FROM harbor.tasn.ir/library/golang:latest as builder
WORKDIR /go/src/gitlab.tasn.ir/ap/miscellaneous-services/post-services
COPY ./go.mod ./
COPY ./go.sum ./
RUN echo "machine gitlab.tasn.ir login mr.rasouli password FBoecriQxqDMT7NxohtQ" > ~/.netrc \
&& go env -w GOSUMDB="off" \
&& go env -w GOPROXY="http://goproxy.tasn.ir" \
&& go env -w GONOPROXY="gitlab.tasn.ir" \
&& go env -w GONOSUMDB="gitlab.tasn.ir" \
&& go env -w GODEBUG="x509ignoreCN=0" \

RUN go mod tidy && go mod download
COPY . .
RUN GOOS=linux go build -a -o ./bin/post-services.out  ./cmd/main.go
# Run pre build files in apline image
FROM harbor.tasn.ir/library/alpine:latest-pack
WORKDIR /post-services
COPY --from=builder /go/src/gitlab.tasn.ir/ap/miscellaneous-services/post-services/bin/post-services.out .

ENTRYPOINT ["./post-services.out"]
