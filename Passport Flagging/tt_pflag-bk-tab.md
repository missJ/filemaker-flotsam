#Tooltip - Booking Tab: Trip Info

##Looks at: Booking, RSTRbkPAXunique

###future trip

	If ( Booking::cd_StartLand ≥ Get ( CurrentDate ) ;

	Case ( 

###departs within 90 days & any rosters missing info

	If ( Booking::cd_StartLand ≤ Get ( CurrentDate ) + 90 ; 

	Let ( [

	  pax = ValueCount ( List ( RSTRbkPAXunique::n_SNBooking ) ) ;
	  exp = ValueCount ( List ( RSTRbkPAXunique::cd_PassportExpires ) )  ;
	  first = ValueCount ( List ( RSTRbkPAXunique::cs_PassportFirst ) )  ;
	  last = ValueCount ( List ( RSTRbkPAXunique::cs_PassportLast ) )  ;
	  nation = ValueCount ( List ( RSTRbkPAXunique::cs_PassportNationality ) )  ;
	  number= ValueCount ( List ( RSTRbkPAXunique::cs_PassportNumber ) )  

	] ;

	If ( pax ≠ exp or pax ≠ first or pax ≠ last or pax ≠ nation or pax ≠ number ; 1 ; "" )

	) ; "" ) ; "One or More Pax is Missing Passport Info" ;



##any non-empty expiration less than 185 days of return


	Let ( after = dJSON ( Booking::cd_EndLand +185 ) ;

	  ExecuteSQL ( "
	  SELECT COUNT ( n_SNConRoster )
	  FROM Roster
	  WHERE n_SNBooking = ?
	  AND cd_PassportExpires IS NOT NULL
	  AND cd_PassportExpires < DATEVAL ( ? )
	  " ; "" ; "" ; Booking::n_SNBooking ; after ) 
	)

	> 0 ; "Check Passport Expiration Dates!" ; "" )

	; "" )