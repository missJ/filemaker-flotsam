#Tooltip - Trips Tab: Main Itin

##Looks at: ITtrip, RSTRtrpTRP

###future trip

	If ( ITtrip::d_Start ≥ Get ( CurrentDate ) ;

	Case ( 

###departs within 90 days & any rosters missing info

	If ( ITtrip::d_Start ≤ Get ( CurrentDate ) + 90 ; 

	Let ( [

	  pax = ValueCount ( List ( RSTRtrpTRP::n_SNBooking ) ) ;
	  exp = ValueCount ( List ( RSTRtrpTRP::cd_PassportExpires ) )  ;
	  first = ValueCount ( List ( RSTRtrpTRP::cs_PassportFirst ) )  ;
	  last = ValueCount ( List ( RSTRtrpTRP::cs_PassportLast ) )  ;
	  nation = ValueCount ( List ( RSTRtrpTRP::cs_PassportNationality ) )  ;
	  number= ValueCount ( List ( RSTRtrpTRP::cs_PassportNumber ) )  

	] ;

	If ( pax ≠ exp or pax ≠ first or pax ≠ last or pax ≠ nation or pax ≠ number ; 1 ; "" )

	) ; "" ) ; "One or More Pax is Missing Passport Info" ;


##any non-empty expiration less than 185 days after return


	Let ( after = dJSON ( ITtrip::cd_End +185 ) ;

	  ExecuteSQL ( "
	  SELECT COUNT ( n_SNConRoster )
	  FROM Roster
	  WHERE n_SNITTrip = ?
	  AND n_SNBooking IS NOT NULL
	  AND cd_PassportExpires IS NOT NULL
	  AND cd_PassportExpires <= DATEVAL ( ? )
	  " ; "" ; "" ; ITtrip::n_SNITtrip ; after ) 

	)

	> 0 ; "Check Passport Expiration Dates!" ; "" )

	; "" )