<?php
################################################################################
#	Class ENUM
#
#		Original code by Mark Manning.
#		Copyrighted (c) 2015 by Mark Manning.
#		All rights reserved.
#
#		This set of code is hereby placed into the free software universe
#		via the GNU greater license thus placing it under the Copyleft
#		rules and regulations with the following modifications:
#
#		1. You may use this work in any other work.  Commercial or otherwise.
#		2. You may make as much money as you can with it.
#		3. You owe me nothing except to give me a small blurb somewhere in
#			your program or maybe have pity on me and donate a dollar to
#			sim_sales@paypal.com.  :-)
#
#	Blurb:
#
#		PHP Class Enums by Mark Manning (markem-AT-sim1-DOT-us).
#		Used with permission.
#
#	Notes:
#
#		VIM formatting.  Set tabs to four(4) spaces.
#
################################################################################
class enum
{
	private $enums;
	private $clear_flag;

################################################################################
#	__construct(). Construction function.  Optionally pass in your enums.
################################################################################
function __construct()
{
	$this->enums = array();
	$this->clear_flag = false;

	if( func_num_args() > 0 ){
		return $this->put( func_get_args() );
		}

	return true;
}
################################################################################
#	put(). Insert one or more enums.
################################################################################
function put()
{
	$args = func_get_args();
#
#	Did they send us an array of enums?
#	Ex: $c->put( array( "a"=>0, "b"=>1,...) );
#	OR  $c->put( array( "a", "b", "c",... ) );
#
	if( is_array($args[0]) ){
#
#	Add them all in
#
		foreach( $args[0] as $k=>$v ){
#
#	Don't let them change it once it is set.
#	Remove the IF statement if you want to be able to modify the enums.
#
			if( !isset($this->enums[$k]) ){
#
#	If they sent an array of enums like this: "a","b","c",... then we have to
#	change that to be "A"=>#. Where "#" is the current count of the enums.
#
				if( is_numeric($k) ){
					$this->enums[$v] = count( $this->enums ) - 1;
					}
#
#	Else - they sent "a"=>"A", "b"=>"B", "c"=>"C"...
#
					else {
						$this->enums[$k] = $v;
						}
				}
			}
		}
#
#	Nope!  Did they just sent us one enum?
#
		else {
#
#	Is this just a default declaration?
#	Ex: $c->put( "a" );
#
			if( count($args) < 2 ){
#
#	Again - remove the IF statement if you want to be able to change the enums.
#
				if( !isset($this->enums[$args[0]]) ){
					$this->enums[$args[0]] = count($this->enums) - 1;
					}
#
#	No - they sent us a regular enum
#	Ex: $c->put( "a", "This is the first enum" );
#
					else {
#
#	Again - remove the IF statement if you want to be able to change the enums.
#
						if( !isset($this->enums[$args[0]]) ){
							$this->enums[$args[0]] = $args[1];
							}
						}
				}
			}

	return true;
}
################################################################################
#	get(). Get one or more enums.
################################################################################
function get()
{
	$num  func_num_args();
	$args = func_get_args();
#
#	Is this an array of enums request? (ie: $c->get(array("a","b","c"...)) )
#
	if( is_array($args[0]) ){
		$ary = array();
		foreach( $args[0] as $k=>$v ){
			$ary[$v] = $this->enums[$v];
			}

		return $ary;
		}
#
#	Is it just ONE enum they want? (ie: $c->get("a") )
#
		else if( ($num > 0) && ($num < 2) ){
			return $this->enums[$args[0]];
			}
#
#	Is it a list of enums they want? (ie: $c->get( "a", "b", "c"...) )
#
		else if( $num > 1 ){
			$ary = array();
			foreach( $args as $k=>$v ){
				$ary[$v] = $this->enums[$v];
				}

			return $ary;
			}
#
#	They either sent something funky or nothing at all.
#
	return false;
}
################################################################################
#	clear(). Clear out the enum array.
#		Optional.  Set the flag in the __construct function.
#		After all, ENUMS are supposed to be constant.
################################################################################
function clear()
{
	if( $clear_flag ){
		unset( $this->enums );
		$this->enums = array();
		}

	return true;
}
################################################################################
#	__call().  In case someone tries to blow up the class.
################################################################################
function __call()
{
	return false;
}
################################################################################
#	__destruct().  Deconstruct the class.  Remove the list of enums.
################################################################################
function __destruct()
{
	unset( $this->enums );
	$this->enums = null;

	return true;
}

}
?>
