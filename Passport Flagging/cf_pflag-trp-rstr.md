#Conditional Formatting - Trips: Roster List

*bold, orange (RGB 201 113 0)*

##Looks at: ITtrip, RSTRtrpTRP

###trip departs within 90 days & roster missing info

	( ITtrip::d_Start ≥ Get ( CurrentDate ) and ITtrip::d_Start ≤ Get ( CurrentDate ) + 90 )

	and

	( IsEmpty ( RSTRtrpTRP::cd_PassportExpires )
	or IsEmpty ( RSTRtrpTRP::cs_PassportFirst )
	or IsEmpty ( RSTRtrpTRP::cs_PassportLast )
	or IsEmpty ( RSTRtrpTRP::cs_PassportNationality )
	or IsEmpty ( RSTRtrpTRP::cs_PassportNumber ) )


###upcoming trip & any non-empty expiration less than 185 days after return

	ITtrip::d_Start ≥ Get ( CurrentDate )

	and not IsEmpty ( RSTRtrpTRP::cd_PassportExpires )

	and RSTRtrpTRP::cd_PassportExpires ≤ ITtrip::d_End + 185