# RLP

## 1.递归长度前缀RLP编码方案

递归长度前缀RLP(Recursive Length Prefix)编码方案是在TelChainBifereum中使用的一种空间有效的对象序列化方案。

规范本身在[黄皮书](http://gavwood.com/paper.pdf)中定义，而下面的页面在[ethereum Wiki](https://github.com/ethereum/wiki/wiki/RLP)中定义。

## 2.RLP类型

RLP编码器定义了两种支持类型：

* string
* list

list类型可以嵌套任意次数，允许对复杂数据结构进行编码。

go-sdk中的[RLP模块](https://github.com/bifj/bifj/tree/master/rlp)提供了RLP编码能力，[RlpEncoderTest](https://github.com/bifj/bifj/blob/master/rlp/src/test/java/org/bifj/rlp/RlpEncoderTest.java)演示了许多不同类型值的编码可以参考:

```
package org.gbif.rlp;

import java.math.BigInteger;
import java.util.Arrays;

import org.junit.Test;

import static org.hamcrest.CoreMatchers.is;
import static org.junit.Assert.assertThat;

public class RlpEncoderTest {

    /**
     * Examples taken from https://github.com/ethereum/wiki/wiki/RLP#examples.

     *
     * <p>For further examples see https://github.com/ethereum/tests/tree/develop/RLPTests.

     */
    @Test
    public void testEncode() {
        assertThat(RlpEncoder.encode(RlpString.create("dog")),
                is(new byte[]{ (byte) 0x83, 'd', 'o', 'g' }));

        assertThat(RlpEncoder.encode(new RlpList(RlpString.create("cat"), RlpString.create("dog"))),
                is(new byte[]{
                        (byte) 0xc8, (byte) 0x83, 'c', 'a', 't', (byte) 0x83, 'd', 'o', 'g'
                }));

        assertThat(RlpEncoder.encode(RlpString.create("")),
                is(new byte[]{ (byte) 0x80 }));

        assertThat(RlpEncoder.encode(RlpString.create(new byte[] {})),
                is(new byte[]{ (byte) 0x80 }));

        assertThat(RlpEncoder.encode(new RlpList()),
                is(new byte[]{ (byte) 0xc0 }));

        assertThat(RlpEncoder.encode(RlpString.create(BigInteger.valueOf(0x0f))),
                is(new byte[]{ (byte) 0x0f }));

        assertThat(RlpEncoder.encode(RlpString.create(BigInteger.valueOf(0x0400))),
                is(new byte[]{ (byte) 0x82, (byte) 0x04, (byte) 0x00 }));

        assertThat(RlpEncoder.encode(new RlpList(
                new RlpList(),
                new RlpList(new RlpList()),
                new RlpList(new RlpList(), new RlpList(new RlpList())))),
                is(new byte[] {
                        (byte) 0xc7,
                        (byte) 0xc0,
                        (byte) 0xc1, (byte) 0xc0,
                        (byte) 0xc3, (byte) 0xc0, (byte) 0xc1, (byte) 0xc0 }));

        assertThat(RlpEncoder.encode(
                RlpString.create("Lorem ipsum dolor sit amet, consectetur adipisicing elit")),
                is(new byte[] {
                        (byte) 0xb8,
                        (byte) 0x38,
                        'L', 'o', 'r', 'e', 'm', ' ', 'i', 'p', 's', 'u', 'm', ' ',
                        'd', 'o', 'l', 'o', 'r', ' ', 's', 'i', 't', ' ',
                        'a', 'm', 'e', 't', ',', ' ',
                        'c', 'o', 'n', 's', 'e', 'c', 't', 'e', 't', 'u', 'r', ' ',
                        'a', 'd', 'i', 'p', 'i', 's', 'i', 'c', 'i', 'n', 'g', ' ',
                        'e', 'l', 'i', 't'
                }));

        assertThat(RlpEncoder.encode(RlpString.create(BigInteger.ZERO)),
                is(new byte[]{ (byte) 0x80 }));

        // https://github.com/paritytech/parity/blob/master/util/rlp/tests/tests.rs#L239
        assertThat(RlpEncoder.encode(RlpString.create(new byte[] { 0 })),
                is(new byte[]{ (byte) 0x00 }));

        assertThat(RlpEncoder.encode(
                new RlpList(
                        RlpString.create("zw"),
                        new RlpList(RlpString.create(4)),
                        RlpString.create(1))),
                is(new byte[]{
                        (byte) 0xc6, (byte) 0x82, (byte) 0x7a, (byte) 0x77, (byte) 0xc1,
                        (byte) 0x04, (byte) 0x01}));

        // 55 bytes. See https://github.com/bifj/bifj/issues/519
        byte[] encodeMe = new byte[55];
        Arrays.fill(encodeMe, (byte) 0);
        byte[] expectedEncoding = new byte[56];
        expectedEncoding[0] = (byte) 0xb7;
        System.arraycopy(encodeMe, 0, expectedEncoding, 1, encodeMe.length);
        assertThat(RlpEncoder.encode(RlpString.create(encodeMe)),
                is(expectedEncoding));
    }
}
</p>
```

## 3.交易编码

在go-sdk中，使用RLP编码将TelChain交易对象编码为字节数组，该字节数组在提交给网络之前被签署。交易类型和签名逻辑位于 `Crypto`模块内，[https://github.com/bifj/bifj/blob/master/crypto/src/test/java/org/bifj/crypto/TransactionEncoderTest.java](https://github.com/bifj/bifj/blob/master/crypto/src/test/java/org/bifj/crypto/TransactionEncoderTest.java)提供交易签名和编码的示例：

```
package org.gbif.crypto;

import java.math.BigInteger;
import java.util.List;

import org.junit.Test;

import org.gbif.rlp.RlpString;
import org.gbif.rlp.RlpType;
import org.gbif.utils.Numeric;

import static org.hamcrest.CoreMatchers.is;
import static org.hamcrest.core.IsEqual.equalTo;
import static org.junit.Assert.assertThat;

public class TransactionEncoderTest {

    @Test
    public void testSignMessage() {
        byte[] signedMessage = TransactionEncoder.signMessage(
                createBiferTransaction(), SampleKeys.CREDENTIALS);
        String hexMessage = Numeric.toHexString(signedMessage);
        assertThat(hexMessage,
                is("0xf85580010a840add5355887fffffffffffffff80"
                        + "1c"
                        + "a046360b50498ddf5566551ce1ce69c46c565f1f478bb0ee680caf31fbc08ab727"
                        + "a01b2f1432de16d110407d544f519fc91b84c8e16d3b6ec899592d486a94974cd0"));
    }

    @Test
    public void testBiferTransactionAsRlpValues() {
        List<rlptype> rlpStrings = TransactionEncoder.asRlpValues(createBiferTransaction(),
                new Sign.SignatureData((byte) 0, new byte[32], new byte[32]));
        assertThat(rlpStrings.size(), is(9));
        assertThat(rlpStrings.get(3), equalTo(RlpString.create(new BigInteger("add5355", 16))));
    }

    @Test
    public void testContractAsRlpValues() {
        List<rlptype> rlpStrings = TransactionEncoder.asRlpValues(
                createContractTransaction(), null);
        assertThat(rlpStrings.size(), is(6));
        assertThat(rlpStrings.get(3), is(RlpString.create("")));
    }

    @Test
    public void testEip155Encode() {
        assertThat(TransactionEncoder.encode(createEip155RawTransaction(), (byte) 1),
                is(Numeric.hexStringToByteArray(
                        "0xec098504a817c800825208943535353535353535353535353535353535353535880de0"
                                + "b6b3a764000080018080")));
    }

    @Test
    public void testEip155Transaction() {
        // https://github.com/ethereum/EIPs/issues/155
        Credentials credentials = Credentials.create(
                "0x4646464646464646464646464646464646464646464646464646464646464646");

        assertThat(TransactionEncoder.signMessage(
                createEip155RawTransaction(), (byte) 1, credentials),
                is(Numeric.hexStringToByteArray(
                        "0xf86c098504a817c800825208943535353535353535353535353535353535353535880"
                                + "de0b6b3a76400008025a028ef61340bd939bc2195fe537567866003e1a15d"
                                + "3c71ff63e1590620aa636276a067cbe9d8997f761aecb703304b3800ccf55"
                                + "5c9f3dc64214b297fb1966a3b6d83")));
    }

    private static RawTransaction createBiferTransaction() {
        return RawTransaction.createBiferTransaction(
                BigInteger.ZERO, BigInteger.ONE, BigInteger.TEN, "0xadd5355",
                BigInteger.valueOf(Long.MAX_VALUE));
    }

    static RawTransaction createContractTransaction() {
        return RawTransaction.createContractTransaction(
                BigInteger.ZERO, BigInteger.ONE, BigInteger.TEN, BigInteger.valueOf(Long.MAX_VALUE),
                "01234566789");
    }

    private static RawTransaction createEip155RawTransaction() {
        return RawTransaction.createBiferTransaction(
                BigInteger.valueOf(9), BigInteger.valueOf(20000000000L),
                BigInteger.valueOf(21000), "0x3535353535353535353535353535353535353535",
                BigInteger.valueOf(1000000000000000000L));
    }
}
</rlptype></rlptype>

package crypto

import (
	"crypto/ecdsa"
	"encoding/hex"
	"errors"
	"github.com/btcsuite/btcutil/base58"
	"io"
	"io/ioutil"
	"math/big"
	"os"
	"strings"

	"github.com/bif/bif-sdk-go/crypto/config"
	"github.com/bif/bif-sdk-go/crypto/secp"
	"github.com/bif/bif-sdk-go/crypto/sm2"
	"github.com/bif/bif-sdk-go/utils"
	"github.com/bif/bif-sdk-go/utils/math"
)

// Keccak256 calculates and returns the Keccak256 hash of the input data.
func Keccak256(cryptoType config.CryptoType, data ...[]byte) []byte {
	switch cryptoType {
	case config.SM2:
		return sm2.Keccak256Sm2(data...)
	case config.SECP256K1:
		return secp.Keccak256Btc(data...)
	default:
		return sm2.Keccak256Sm2(data...)
	}
}

// Keccak256Hash calculates and returns the Keccak256 hash of the input data,
// converting it to an internal Hash data structure.
func Keccak256Hash(cryptoType config.CryptoType, data ...[]byte) (h utils.Hash) {
	switch cryptoType {
	case config.SM2:
		return sm2.Keccak256HashSm2(data...)
	case config.SECP256K1:
		return secp.Keccak256HashBtc(data...)
	default:
		return sm2.Keccak256HashSm2(data...)
	}
}

// ToECDSA creates a private key with the given D value.
func ToECDSA(d []byte, cryptoType config.CryptoType) (*ecdsa.PrivateKey, error) {
	switch cryptoType {
	case config.SM2:
		return sm2.ToECDSASm2(d, true)
	case config.SECP256K1:
		return secp.ToECDSABtc(d, true)
	default:
		return sm2.ToECDSASm2(d, true)
	}
}

// ToECDSAUnsafe blindly converts a binary blob to a private key. It should almost
// never be used unless you are sure the input is valid and want to avoid hitting
// errors due to bad origin encoding (0 prefixes cut off).
func ToECDSAUnsafe(d []byte, cryptoType config.CryptoType) *ecdsa.PrivateKey {
	var priv *ecdsa.PrivateKey
	switch cryptoType {
	case config.SM2:
		priv, _ = sm2.ToECDSASm2(d, false)
	case config.SECP256K1:
		priv, _ = secp.ToECDSABtc(d, false)
	default:
		priv, _ = sm2.ToECDSASm2(d, false)
	}
	return priv
}

// FromECDSA exports a private key into a binary dump.
func FromECDSA(priv *ecdsa.PrivateKey) []byte {
	if priv == nil {
		return nil
	}
	return math.PaddedBigBytes(priv.D, priv.Params().BitSize/8)
}

// UnmarshalPubkey converts bytes to a secp256k1 public key.
func UnmarshalPubkey(pub []byte) (*ecdsa.PublicKey, error) {

	if pub[0] != 4 { // uncompressed form
		return nil, errors.New("invalid pub byte")
	}
	x := new(big.Int).SetBytes(pub[1:33])
	y := new(big.Int).SetBytes(pub[33:])

	if sm2.S256Sm2().IsOnCurve(x, y) {
		return sm2.UnmarshalPubkeySm2(pub)
	}
	if secp.S256Btc().IsOnCurve(x, y) {
		return secp.UnmarshalPubkeyBtc(pub)
	}
	return sm2.UnmarshalPubkeySm2(pub)
}

func FromECDSAPub(p *ecdsa.PublicKey) []byte {
	if sm2.S256Sm2().IsOnCurve(p.X, p.Y) {
		return sm2.FromECDSAPubSm2(p)
	}
	if secp.S256Btc().IsOnCurve(p.X, p.Y) {
		return secp.FromECDSAPubBtc(p)
	}
	return sm2.FromECDSAPubSm2(p)
}

// HexToECDSA parses a secp256k1 private key.
func HexToECDSA(hexkey string, cryptoType config.CryptoType) (*ecdsa.PrivateKey, error) {
	b, err := hex.DecodeString(hexkey)
	if err != nil {
		return nil, errors.New("invalid hex string")
	}
	return ToECDSA(b, cryptoType)
}

// LoadECDSA loads a secp256k1 private key from the given file.
func LoadECDSA(file string, cryptoType config.CryptoType) (*ecdsa.PrivateKey, error) {
	buf := make([]byte, 64)
	fd, err := os.Open(file)
	if err != nil {
		return nil, err
	}
	defer fd.Close()
	if _, err := io.ReadFull(fd, buf); err != nil {
		return nil, err
	}

	key, err := hex.DecodeString(string(buf))
	if err != nil {
		return nil, err
	}
	return ToECDSA(key, cryptoType)
}

// SaveECDSA saves a secp256k1 private key to the given file with
// restrictive permissions. The key data is saved hex-encoded.
func SaveECDSA(file string, key *ecdsa.PrivateKey) error {
	k := hex.EncodeToString(FromECDSA(key))
	return ioutil.WriteFile(file, []byte(k), 0600)
}

func GenerateKey(cryptoType config.CryptoType) (*ecdsa.PrivateKey, error) {
	switch cryptoType {
	case config.SM2:
		return sm2.GenerateKeySm2()
	case config.SECP256K1:
		return secp.GenerateKeyBtc()
	default:
		return sm2.GenerateKeySm2()
	}
}

func PubkeyToAddress(p ecdsa.PublicKey) utils.Address {
	length := config.HashLength
	var publicKey []byte
	var hash []byte
	var prefix strings.Builder

	if sm2.S256Sm2().IsOnCurve(p.X, p.Y) {
		publicKey = sm2.CompressPubkeySm2(&p)
		hash = sm2.Keccak256Sm2(publicKey)
		prefix.WriteString(string(config.SM2_Prefix))
	} else if secp.S256Btc().IsOnCurve(p.X, p.Y) {
		publicKey = secp.CompressPubkeyBtc(&p)
		hash = secp.Keccak256Btc(publicKey)
		prefix.WriteString(string(config.SECP256K1_Prefix))
	} else {
		publicKey = sm2.CompressPubkeySm2(&p)
		hash = sm2.Keccak256Sm2(publicKey)
		prefix.WriteString(string(config.SM2_Prefix))
	}

	hashLength := len(hash)
	if hashLength < length {
		length = hashLength
	}
	h := hash[hashLength-length:]

	var encode string
	encode = base58.Encode(h)
	prefix.WriteString(string(config.BASE58_Prefix))

	return utils.StringToAddress(prefix.String() + encode)
}

```

## 4.依赖关系

这是一个非常轻量级的模块，没有其他依赖项。

