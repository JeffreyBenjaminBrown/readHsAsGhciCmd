module ReadHsAsGhciCmd where

import Control.Applicative
import Data.Void (Void)
import Text.Megaparsec
import Text.Megaparsec.Char (satisfy, string, space, space1, anyChar, tab)


main :: FilePath -> IO String
main filename = do
  s <- readFile filename
  case hsToGhci s of Left e -> (putStrLn $ show e) >> return ""
                     Right s -> return s

hsToGhci :: String -> Either (ParseError (Token String) Void) String
hsToGhci s = do s1 <- mapM (parse line "") $ lines s
                let s2 = filter (not . ignorable) s1
                    f (Start s) = [":}",":{",s]
                    f (More s) = [s]
                    s3 = concatMap f s2
                return $ unlines $ tail s3 ++ [":}"]


-- | == the Line type

data Line = Ignore | Start String | More String deriving Show

ignorable :: Line -> Bool
ignorable Ignore = True
ignorable _ = False


-- | == parsing Lines

type Parser = Parsec Void String

line = foldl1 (<|>) $ map try [emptyLine,comment,start,more]

emptyLine :: Parser Line
emptyLine = space >> eof >> return Ignore

comment :: Parser Line
comment = space >> satisfy (== '-') >> satisfy (== '-')
  >> skipMany anyChar >> eof >> return Ignore

start :: Parser Line
start = do c <- satisfy (/= ' ')
           rest <- many anyChar
           return $ Start $ c : rest

more :: Parser Line
more = do c <- satisfy (== ' ')
          rest <- many anyChar
          return $ More $ c : rest
