##不兼容

###json_decode() strictness
	json_decode() now rejects non-lowercase variants of the JSON literals true, false and null at all times, as per the JSON specification, and sets json_last_error() accordingly. Previously, inputs to json_decode() that consisted solely of one of these values in upper or mixed case were accepted.

	This change will only affect cases where invalid JSON was being passed to json_decode(): valid JSON input is unaffected and will continue to be parsed normally.

###Stream wrappers now verify peer certificates and host names by default when using SSL/TLS

	All encrypted client streams now enable peer verification by default. By default, this will use OpenSSL's default CA bundle to verify the peer certificate. In most cases, no changes will need to be made to communicate with servers with valid SSL certificates, as distributors generally configure OpenSSL to use known good CA bundles.

	The default CA bundle may be overridden on a global basis by setting either the openssl.cafile or openssl.capath configuration setting, or on a per request basis by using the cafile or capath context options.

	While not recommended in general, it is possible to disable peer certificate verification for a request by setting the verify_peer context option to FALSE, and to disable peer name validation by setting the verify_peer_name context option to FALSE.

###GMP resources are now objects

	GMP resources are now objects. The functional API implemented in the GMP extension has not changed, and code should run modified unless it checks explicitly for a resource using is_resource() or similar.

###Mcrypt functions now require valid keys and IVs

	mcrypt_encrypt(), mcrypt_decrypt(), mcrypt_cbc(), mcrypt_cfb(), mcrypt_ecb(), mcrypt_generic() and mcrypt_ofb() will no longer accept keys or IVs with incorrect sizes, and block cipher modes that require IVs will now fail if an IV isn't provided.