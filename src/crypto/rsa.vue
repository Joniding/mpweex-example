<template>
    <scroller class="wrapper" show-scrollbar="false">
        <div style="height: 40px;"></div>
        <div class="title_root">
            <div class="tdot"></div>
            <text class="tlab">RSA Hex</text>
        </div>
        <div class="pannel">
            <text class="content">{{hexBuf}}</text>
        </div>
        <div class="title_root">
            <div class="tdot"></div>
            <text class="tlab">RSA Base64</text>
        </div>
        <div class="pannel">
            <text class="content">{{base64Buf}}</text>
        </div>
        <div class="title_root">
            <div class="tdot"></div>
            <text class="tlab">RSA UrlSafeBase64</text>
        </div>
        <div class="pannel">
            <text class="content">{{urlSafeBase64Buf}}</text>
        </div>
        <div style="height: 40px;"></div>
    </scroller>
</template>

<script>
    const crypto = weex.requireModule("crypto");
    module.exports = {
        data: {
            hexBuf: 'SDK版本太低，不支持',
            base64Buf: 'SDK版本太低，不支持',
            urlSafeBase64Buf: 'SDK版本太低，不支持',
            resultTypes: ['hex', 'base64', 'urlSafeBase64']
        },
        created() {
            if(parseInt(WXEnvironment.weexVersionCode) < 2802) {
                // SDK版本太低
                return;
            }
            for(let idx in this.resultTypes) {
                this.runAlg(this.resultTypes[idx]);
            }
        },
        methods: {
            runAlg(resultType, outBuf) {
                let pemKey = "-----BEGIN RSA PRIVATE KEY-----\n" +
                    "MIICXQIBAAKBgQC1d8wAeHL1FaSOi7DVCOGmvTHDcjpZHnqv8GbWB5g1S0LJnymN\n" +
                    "xuLIhTWzszOxiAqGET4rw3sltai7lcAGaBB2PZpY0CU+P8ppmC+8uTlAI0TdPLhm\n" +
                    "+WqGyyUu3VwwJR48/WhK/0bsUOyjwZi3CQe80TRfnG/EcgehH4GxLbqz5wIDAQAB\n" +
                    "AoGBAKcf9lR0mcLXtN7HDguVC2Spl5wdplkPNgS1DbCN/AMRFihkGjwFcDUmYafn\n" +
                    "IXOeC7sfRDe/57l6DTT9nIUJ8CW3iyTWaYJJel0VNO0RQikYUyIQtBhttaisiszp\n" +
                    "aAQpAYZEavre570nKUCHbnnrmaC93PFbfwdKinQ5BdMmD+UZAkEA5gnNvrN2VCE7\n" +
                    "uMKEuLNQ2xQT7E+cgjp3eOjuzZGD0VVXme4lnNSc1QIgOAcU9MuNzoWzXeUhDLPv\n" +
                    "dZnNu/uBfQJBAMnytrkLCJgL+ggVvx/vKtV3p6AHXnEA3A3g4jgbVqimDBvdIIdS\n" +
                    "u5gjByeP58YeTAyp6tD2awvGQqEn0Z0kCDMCQA+tB1pBfITLJvi2OLklbxMe0SS/\n" +
                    "YBj3xwB0TyGvEt6HBEs3EVUYn/9b/7oRsXnlDSrPraNuY8wrztuiuYRf5TkCQQCd\n" +
                    "oxVYyjESJr8sknUXY2TnLritJTNmOEqNls5fB5AUo1DuayTaHQ2MS0NpcV51eu7Y\n" +
                    "L8a5CLE0hrU6ANARvq+bAkAYXxf1CpMKjJAoT5Sx0jvCcQKZlS2fmoMfJjv/qy0h\n" +
                    "kmZro0KkK1TAB3QWUk3TPOe44YLk1/F13XWYeRIeg5g+\n" +
                    "-----END RSA PRIVATE KEY-----";

                let data = "B577CC007872F515A48E8BB0D508E1A6BD31C3723A591E7AAFF066D60798354B42C99F29" +
                    "8DC6E2C88535B3B333B1880A86113E2BC37B25B5A8BB95C0066810763D9A58D0253E3FCA69982FBCB93940" +
                    "2344DD3CB866F96A86CB252EDD5C30251E3CFD684AFF46EC50ECA3C198B70907BCD1345F9C6FC47207A11F" +
                    "81B12DBAB3E7010001"

                crypto.rsa({
                    algorithm: 'RSA/None/PKCS1Padding',
                    pemkey: pemKey,
                    forEncryption: true,
                    data: data,
                    keyType: 'hex',
                    contentType: 'hex',
                    resultType: resultType
                }, (e) => {
                    // 检查hex输出
                    if(resultType == 'hex' && e.data != '8E4D5AE64E9A01569C8610E8841A3502E9687ABC1F0240FD3CA2627EA8F31253E57944E7952BDA1A6ED7CC123E13166887585EC511D47E379B94332510AA8537C02B3DF0437ECC031CD0797BD229E220B2471ADA934F3499A5C1E1DCFC418DDF4B4FD4CD42229BD79B41F32F59529C00A455610A4AEF3EF855824CFF0CC93A0B1C6667842E36BD080DE5F774D8CC9F84A8511AE8B400C8B4DF9AB7E4DE87D2059377BB946F7E4E3AD1BFD5E32D048CB756B7D9A23849BC7CD0D44C420DD272D8168D9123D85A1604C2E5BB698A64FE5F8E252F86AA98EB4840621254FF3566F01A669300BE71EE1F5ADB1CCDE9A11CE6CEE19653780F58F6C0D3302E5E2A2F70') {
                        this[resultType + "Buf"] = '计算异常\n期望值:8E4D5AE64E9A01569C8610E8841A3502E9687ABC1F0240FD3CA2627EA8F31253E57944E7952BDA1A6ED7CC123E13166887585EC511D47E379B94332510AA8537C02B3DF0437ECC031CD0797BD229E220B2471ADA934F3499A5C1E1DCFC418DDF4B4FD4CD42229BD79B41F32F59529C00A455610A4AEF3EF855824CFF0CC93A0B1C6667842E36BD080DE5F774D8CC9F84A8511AE8B400C8B4DF9AB7E4DE87D2059377BB946F7E4E3AD1BFD5E32D048CB756B7D9A23849BC7CD0D44C420DD272D8168D9123D85A1604C2E5BB698A64FE5F8E252F86AA98EB4840621254FF3566F01A669300BE71EE1F5ADB1CCDE9A11CE6CEE19653780F58F6C0D3302E5E2A2F70\n实际值:' + e.data;
                    }else {
                        this[resultType + "Buf"] = e.data;
                    }
                });
            }
        }
    }
</script>

<style scoped>
    .wrapper {
        background-color: white;
    }
    .title_root {
        flex-direction: row;
        align-items: center;
        padding-left: 40px;
        padding-right: 40px;
    }
    .tdot {
        width: 20px;
        height: 20px;
        background-color: #0088fb;
        border-radius: 10px;
    }
    .tlab {
        font-size: 32px;
        font-weight: bold;
        margin-left: 20px;
    }
    .pannel {
        flex-direction: column;
        margin-top: 20px;
        margin-bottom: 20px;
        background-color: #F2F2F2;
        border-radius: 10px;
        padding: 40px;
        margin-left: 40px;
        margin-right: 40px;
    }
    .content {
        font-size: 26px;
        color: #666;
    }
</style>