##新特性

###Constant scalar expressions
	It is now possible to provide a scalar expression involving numeric and string literals and/or constants in contexts where PHP previously expected a static value, such as constant and property declarations and default function arguments.

	<?php
	const ONE = 1;
	const TWO = ONE * 2;

	class C {
		const THREE = TWO + 1;
		const ONE_THIRD = ONE / self::THREE;
		const SENTENCE = 'The value of THREE is '.self::THREE;

		public function f($a = ONE + self::THREE) {
			return $a;
		}
	}

	echo (new C)->f()."\n";
	echo C::SENTENCE;
	?>
	以上例程会输出：

	4
	The value of THREE is 3
	
###Variadic functions via ...

	Variadic functions can now be implemented using the ... operator, instead of relying on func_get_args().

	<?php
	function f($req, $opt = null, ...$params) {
		// $params is an array containing the remaining arguments.
		printf('$req: %d; $opt: %d; number of params: %d'."\n",
			   $req, $opt, count($params));
	}

	f(1);
	f(1, 2);
	f(1, 2, 3);
	f(1, 2, 3, 4);
	f(1, 2, 3, 4, 5);
	?>
	以上例程会输出：

	$req: 1; $opt: 0; number of params: 0
	$req: 1; $opt: 2; number of params: 0
	$req: 1; $opt: 2; number of params: 1
	$req: 1; $opt: 2; number of params: 2
	$req: 1; $opt: 2; number of params: 3
	
###Argument unpacking via ...

	Arrays and Traversable objects can be unpacked into argument lists when calling functions by using the ... operator. This is also known as the splat operator in other languages, including Ruby.

	<?php
	function add($a, $b, $c) {
		return $a + $b + $c;
	}

	$operators = [2, 3];
	echo add(1, ...$operators);
	?>
	以上例程会输出：

	6
	
###Exponentiation via **

	A right associative ** operator has been added to support exponentiation, along with a **= shorthand assignment operator.

	<?php
	printf("2 ** 3 ==      %d\n", 2 ** 3);
	printf("2 ** 3 ** 2 == %d\n", 2 ** 3 ** 2);

	$a = 2;
	$a **= 3;
	printf("a ==           %d\n", $a);
	?>
	以上例程会输出：

	2 ** 3 ==      8
	2 ** 3 ** 2 == 512
	a ==           8
	
###use function and use const

	The use operator has been extended to support importing functions and constants in addition to classes. This is achieved via the use function and use const constructs, respectively.

	<?php
	namespace Name\Space {
		const FOO = 42;
		function f() { echo __FUNCTION__."\n"; }
	}

	namespace {
		use const Name\Space\FOO;
		use function Name\Space\f;

		echo FOO."\n";
		f();
	}
	?>
	以上例程会输出：

	42
	Name\Space\f
	
###phpdbg

	PHP now includes an interactive debugger called phpdbg implemented as a SAPI module. For more information, please visit the ? phpdbg documentation.

###Default character encoding

	default_charset is now used as the default character set for functions that are encoding-specific, such as htmlspecialchars(). Note that if the (now deprecated) iconv and mbstring encoding settings are set, they will take precedence over default_charset.

	The default value for this setting is UTF-8.

###php://input is reusable

	php://input may now be reopened and read as many times as required. This work has also resulted in a major reduction in the amount of memory required to deal with POST data.

###Large file uploads

	Files larger than 2 gigabytes in size are now accepted.

###GMP supports operator overloading

	GMP objects now support operator overloading and casting to scalar types. This allows for more expressive code using GMP:

	<?php
	$a = gmp_init(42);
	$b = gmp_init(17);

	// Pre-5.6 code:
	var_dump(gmp_add($a, $b));
	var_dump(gmp_add($a, 17));
	var_dump(gmp_add(42, $b));

	// New code:
	var_dump($a + $b);
	var_dump($a + 17);
	var_dump(42 + $b);
	?>
	
###hash_equals() for timing attack safe string comparison

	The hash_equals() function has been added to compare two strings in constant time. This should be used to mitigate timing attacks; for instance, when testing crypt() password hashes (assuming that you are unable to use password_hash() and password_verify(), which aren't susceptible to timing attacks).

	<?php
	$expected  = crypt('12345', '$2a$07$usesomesillystringforsalt$');
	$correct   = crypt('12345', '$2a$07$usesomesillystringforsalt$');
	$incorrect = crypt('1234',  '$2a$07$usesomesillystringforsalt$');

	var_dump(hash_equals($expected, $correct));
	var_dump(hash_equals($expected, $incorrect));
	?>
	以上例程会输出：

	bool(true)
	bool(false)
	
###gost-crypto hash algorithm

	The gost-crypto hash algorithm has been added. This implements the GOST hash function using the CryptoPro S-box tables as specified by ? RFC 4357, section 11.2.

###SSL/TLS improvements

	A wide range of improvements have been made to the SSL/TLS support in PHP 5.6. These include enabling peer verification by default, supporting certificate fingerprint matching, mitigating against TLS renegotiation attacks, and many new SSL context options to allow more fine grained control over protocol and verification settings when using encrypted streams.

	These changes are described in more detail in the OpenSSL changes in PHP 5.6.x section of this migration guide.
	